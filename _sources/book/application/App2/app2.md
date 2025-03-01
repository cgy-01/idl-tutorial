# Spectral Data Extraction and Feature Extraction
## Overview

- Smoothing filter
- Calculate red edge position
- Calculate red edge amplitude
- Calculate red edge area

## Data
Spectral data of ground objects `S0*.txt`(* from 1 to 5)
||A|B|
|:-:|:-:|:-:|
|1|Wavelength|Reflectance|
|2|350|0.0274|
|3|351|0.0269|
|4|352|0.0267|
|...|...|...|
## Code
function `Read_file`
```idl
function Read_file
    work_dir = dialog_pickfile(title='choose path', /directory)
    cd, work_dir
    fns = file_search('*.txt', count=fnums)
    
    return, {fns:fns, fnums:fnums}
end 
```
function `SG_smooth`
```idl
function SG_smooth, ref, Nleft, Nright, order, degree
    SG_filter = savgol(Nleft, Nright, order, degree)
    ref_smooth = convol(transpose(ref), SG_filter, /edge_truncate)

    return, ref_smooth
end
```
function `Data_Deriv`
```idl
function Data_Deriv, wv, ref_smooth
    deriv_first = deriv(wv, ref_smooth)
    
    w = where(wv ge 680 and wv le 760)
    twv = wv[w]
    tderiv = deriv_first[w]

    RE_pos = max(tderiv, index)
    RE_amp = twv(index)

    intvl_spec = twv[1] - twv[0]
    RE_are = total(tderiv*intvl_spec)

    return, {deriv_first:deriv_first,RE_par:[RE_pos,RE_amp,RE_are]}
end
```
function `Save_file`
```idl
pro Save_file, fnums, fns, wv, ref_smooth, deriv_first, RE_pars
    snames = strarr(fnums)
    for i=0,fnums-1 do snames[i]=file_basename(fns[i], '.txt')
    header = ['Wavelength(nm)', snames]

    o_fn = 'Spectra_smoothed.csv'
    help, [wv, ref_smooth]
    help, header
    write_csv, o_fn, [wv, ref_smooth], header=header

    o_fn = 'Deriv_1st.csv'
    write_csv, o_fn, [wv, deriv_first], header=header

    o_fn = 'Rededge_pars.csv'
    write_csv, o_fn, snames, transpose(RE_pars[0,*]), $
        transpose(RE_pars[1,*]),  transpose(RE_pars[2,*]), $
        header=['Par','REP','Dr','SDr']
end
```
procedure 
```idl
pro test2
    sfile = Read_file()
    fns = sfile.fns
    fnums = sfile.fnums

    nl = file_lines(fns[0]) - 1
    ref_smooth = fltarr(fnums, nl)
    deriv_first = fltarr(fnums, nl)
    RE_pars = fltarr(3, fnums)

    for i=0, fnums-1 do begin
      header = ''
      data = fltarr(2, nl)
      
      openr, lun, fns[i], /get_lun
      readf, lun, header
      readf, lun, data
      free_lun, lun
    
      wv = data[0, *]
      ref = data[1, *]
      ref_smooth_i = SG_smooth(ref, 5, 5, 0, 2)
      ref_smooth[i, *] = ref_smooth_i
      
      Out = Data_Deriv(wv, ref_smooth_i)
      deriv_first[i,*] = Out.deriv_first
      RE_pars[*, i] = Out.RE_par
    endfor

    Save_file, fnums, fns, wv, ref_smooth, deriv_first, RE_pars

end
```
## Result

file `Spectra_smoothed.csv`

|Wavelength(nm)|S01|S02|S03|S04|S05|
|-|-|-|-|-|-|
|350.000|0.0270858|0.0270858|0.0270858|0.0270858|0.0270858|
|351.000|0.0270392|0.0270392|0.0270392|0.0270392|0.0270392|
|352.000|0.0268846|0.0268846|0.0268846|0.0268846|0.0268846|
|...|...|...|...|...|...|

file `Deriv_1st.csv`

|Wavelength(nm)|S01|S02|S03|S04|S05|
|-|-|-|-|-|-|
|350.000|7.34348e-006|7.34348e-006|7.34348e-006|7.34348e-006|7.34348e-006|
|351.000|-0.000100583|-0.000100583|-0.000100583|-0.000100583|-0.000100583|
|352.000|-0.000158509|-0.000158509|-0.000158509|-0.000158509|-0.000158509|
|...|...|...|...|...|...|

file `Rededge_pars.csv`

|Par|REP|Dr|SDr|
|-|-|-|-|
|S01|0.0139338|730.000|0.654881|
|S02|0.0139338|730.000|0.654881|
|S03|0.0139338|730.000|0.654881|
|S04|0.0139338|730.000|0.654881|
|S05|0.0139338|730.000|0.654881|