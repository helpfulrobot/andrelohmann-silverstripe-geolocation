# silverstripe geolocation

## Maintainers

 * Andre Lohmann (Nickname: andrelohmann)
  <lohmann dot andre at googlemail dot com>

## Requirements

Silverstripe 3.2.x

## Introduction

GeoLocation FieldTypes - allow calculations on geolocations.

## Installation

geolocation needs a UDF (mysql user defined function) to be created
either create this function manually with the following SQL Statement (copy/paste to phpmyadmin)
```
DROP FUNCTION IF EXISTS geodistance;
delimiter //
CREATE FUNCTION geodistance (lat1 DOUBLE, lng1 DOUBLE, lat2 DOUBLE, lng2 DOUBLE) RETURNS DOUBLE NO SQL
BEGIN
DECLARE radius DOUBLE;
DECLARE distance DOUBLE;
DECLARE vara DOUBLE;
DECLARE varb DOUBLE;
DECLARE varc DOUBLE;
SET lat1 = RADIANS(lat1);
SET lng1 = RADIANS(lng1);
SET lat2 = RADIANS(lat2);
SET lng2 = RADIANS(lng2);
SET radius = 6371.0;
SET varb = SIN((lat2 - lat1) / 2.0);
SET varc = SIN((lng2 - lng1) / 2.0);
SET vara = SQRT((varb * varb) + (COS(lat1) * COS(lat2) * (varc * varc)));
SET distance = radius * (2.0 * ASIN(CASE WHEN 1.0 < vara THEN 1.0 ELSE vara END));
RETURN distance;
END;
//
delimiter ;
```
or on each /dev/build by adding the following line to your _ss_environment.php
```
define('CREATE_GEODISTANCE_UDF', true);
```

Set the Google Api Key inside your _ss_environment.php if necessary
```
define('GOOGLE_MAPS_API_KEY', '__YOUR_KEY__');
```
