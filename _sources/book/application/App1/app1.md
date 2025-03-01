# Temperature Mobile Observation Data Processing
## Overview
Today we use portable mobile weather station and handheld GNSS device to conduct temperature movement observations.The weather station acquires weather and time information, and GNSS obtain the latitude, longitude and time information of the observation route.  
You need to correlate temperature and latitude and longitude information according to the time, and finally product temperature data with latitude and longitude.
- Read Data
- Interpolate
- Save Output
## Data
Temperature Observation File `Ta_record.csv`

||A|B|
|-|-|-|
|1|Data/Time|Temperature|
|2|20221108 14:15:01|23.58|
|3|20221108 14:15:03|23.61|
|4|20221108 14:15:05|23.61|
|...|...|...|

GNSS Data File `GNSS_record.csv`

||A|B|C|D|
|-|-|-|-|-|
|1|Data|Time|Lat|Lon|
|2|11/8/2022|14:14:32|32.207723|118.7126563|
|3|11/8/2022|14:14:33|32.2077237|118.7126553|
|4|11/8/2022|14:14:34|32.2077243|118.7126544|
|...|...|...|...|...|

## Code
Procedure `ReadData`
```idl
pro ReadData, Datetime_Ta=Datetime_Ta, Ta=Ta, Date_GNSS=Date_GNSS, Time_GNSS=Time_GNSS, Lat=Lat, Lon=Lon
  fn_Ta = dialog_pickfile(title='chose temperature file', get_path=work_dir)
  cd, work_dir
  fn_GNSS = dialog_pickfile(title='chose GNSS file')

  data_Ta = read_csv(fn_Ta, count=n11)
  data_GNSS = read_csv(fn_GNSS, count=n12)

  ; help, data_Ta
  ; help, data_GNSS
  Datetime_Ta = data_Ta.(0)
  Ta = data_Ta.(1)
  Date_GNSS = data_GNSS.(0)
  Time_GNSS = data_GNSS.(1)
  Lat = data_GNSS.(2)
  Lon = data_GNSS.(3)
end
```
Procedure `ConvertJultime`
```idl
pro ConvertJultime, Datetime_Ta, Date_GNSS, Time_GNSS, jultime_Ta=jultime_Ta, jultime_GNSS=jultime_GNSS, date_Ta=date_Ta, time_Ta=time_Ta
  ; 20221108 14:15:01
  n_Ta = n_elements(Datetime_Ta)
  jultime_Ta = dblarr(n_Ta)

  date_Ta = strarr(n_Ta)
  time_Ta = strarr(n_Ta)

  for i=0, n_Ta - 1 do begin
    tstr = strsplit(Datetime_Ta[i], ' :', /extract)
    ;year = tstr[0][0:3]  syntax wrong
    ;month = tstr[0][4:5] syntax wrong
    ;day = tstr[0][6:7]   syntax wrong
    year = strmid(tstr[0], 0, 4)    ; 提取年份
    month = strmid(tstr[0], 4, 2)   ; 提取月份
    day = strmid(tstr[0], 6, 2)     ; 提取日期
    hour = tstr[1]
    minute = tstr[2]
    second = tstr[3]

    jultime_Ta[i] = julday(month, day, year, hour, minute, second)

    tstr = strsplit(Datetime_Ta[i], ' ', /extract)
    date_Ta[i] = tstr[0]
    time_Ta[i] = tstr[1]
  endfor

  n_GNSS = n_elements(Date_GNSS)
  jultime_GNSS = dblarr(n_GNSS)

  for i=0, n_GNSS - 1 do begin
    tstr = strsplit(Date_GNSS[i], '/', /extract)
    month = tstr[0]
    day = tstr[1]
    year = tstr[2]

    tstr = strsplit(Time_GNSS[i], ':', /extract)
    hour = tstr[0]
    minute = tstr[1]
    second = tstr[2]

    jultime_GNSS[i] = julday(month, day, year, hour, minute, second)
  endfor

end
```
Procedure `Interpolation`
```idl
pro Interpolation, Lat, Lon, time_Ta, time_GNSS, Lat_Ta=Lat_Ta, Lon_Ta=Lon_Ta
  Lat_Ta = interpol(Lat, time_GNSS, time_Ta)
  Lon_Ta = interpol(Lon, time_GNSS, time_Ta)
end
```
Procedure `SaveFile`
```idl
pro SaveFile, Date_Ta, Time_Ta, Lat_Ta, Lon_Ta, Ta
  o_fn = dialog_pickfile(title='Save Output') + '.csv'
  header = ['Date', 'Time', 'Lat', 'Lon', 'Ta']
  write_csv, o_fn, Date_Ta, Time_Ta, Lat_Ta, Lon_Ta, Ta, header = header
end
```
Main Procedure `TemperatureMobileObservations`
```idl
pro main
  ReadData, Datetime_Ta=Datetime_Ta, Ta=Ta, Date_GNSS=Date_GNSS, Time_GNSS=Time_GNSS, Lat=Lat, Lon=Lon
  ConvertJultime, Datetime_Ta, Date_GNSS, Time_GNSS, jultime_Ta=jultime_Ta, jultime_GNSS=jultime_GNSS, date_Ta=date_Ta, time_Ta=time_Ta
  Interpolation, Lat, Lon, jultime_Ta, jultime_GNSS, Lat_Ta=Lat_Ta, Lon_Ta=Lon_Ta
  SaveFile, date_Ta, time_Ta, Lat_Ta, Lon_Ta, Ta
end
```

# Result
file `output.csv`
|Date|Time|Lat|Lon|Ta|
|-|-|-|-|-
|20221108|14:15:01|32.20772737000000|118.7126562000000|23.58000000000000|
|20221108|14:15:03|32.20772734000000|118.7126563000000|23.61000000000000|
|20221108|14:15:05|32.20772722000000|118.7126561000000|23.61000000000000|