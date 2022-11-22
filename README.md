
[![Arduino CI](https://github.com/RobTillaart/pressure/workflows/Arduino%20CI/badge.svg)](https://github.com/marketplace/actions/arduino_ci)
[![Arduino-lint](https://github.com/RobTillaart/pressure/actions/workflows/arduino-lint.yml/badge.svg)](https://github.com/RobTillaart/pressure/actions/workflows/arduino-lint.yml)
[![JSON check](https://github.com/RobTillaart/pressure/actions/workflows/jsoncheck.yml/badge.svg)](https://github.com/RobTillaart/pressure/actions/workflows/jsoncheck.yml)
[![License: MIT](https://img.shields.io/badge/license-MIT-green.svg)](https://github.com/RobTillaart/pressure/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/release/RobTillaart/pressure.svg?maxAge=3600)](https://github.com/RobTillaart/pressure/releases)


# pressure

Arduino library for pressure conversion.


## Description

Simple library to convert between several pressure formats.
It consists of a number of setters and getters and internally it uses millibar. 
In fact it just hides all conversion constants.

Pressure is implemented as a float so this limits the precision of the value.

Note: as the conversion is 2 steps the conversion error might be larger than in a single conversion step.

Note: constants need to be verified.


## Interface


#### Constructor

- **pressure(float value = 0.0)** Constructor, with optional initial value.


#### setters

- **void setMilliBar(float value)** sets pressure in milliBar.
- **void setBar(float value)** sets pressure in bar.
- **void setPSI(float value)** sets pressure in PSI = Pounds per Square Inch.
- **void setATM(float value)** sets pressure in Atmosphere.
- **void setDynes(float value)** sets pressure in Dynes.
- **void setInchHg(float value)** sets pressure in inches mercury.
- **void setInchH2O(float value)** sets pressure in inches water.
- **void setPascal(float value)** sets pressure in Pascal. Note this is the **SI** unit.
- **void setTORR(float value)** sets pressure in TORR.
- **void setCmHg(float value)** sets pressure in centimetre mercury.
- **void setCmH2O(float value)** sets pressure in centimetre water.
- **void setMSW(float value)** sets pressure in Meters of Sea Water. (under water pressure unit).


#### getters

- **float getMilliBar()** returns pressure in milliBar.
- **float getBar()** returns pressure in bar.
- **float getPSI()** returns pressure in PSI = Pounds per Square Inch.
- **float getATM()** returns pressure in Atmosphere.
- **float getDynes()** returns pressure in Dynes.
- **float getInchHg()** returns pressure in inches mercury.
- **float getInchH2O()** returns pressure in inches water.
- **float getPascal()** returns pressure in Pascal. Note this is the **SI** unit.
- **float getTORR()** returns pressure in TORR.
- **float getCmHg()** returns pressure in centimetre mercury.
- **float getCmH2O()** returns pressure in centimetre water.
- **float getMSW()** returns pressure in Meters of Sea Water. (under water pressure unit).


#### Gas law (experimental see below)

- **change(float T1, float T2, float V1 = 1, float V2 = 1, float N1 = 1, float N2 = 1)**
apply changing temperature (**Kelvin**), volume (m3) and moles. If an parameter does not change
just fill in the value 1 for both before and after.


#### constants

The library has a number of constants to convert units.
These constants can be used to write specific convertors or define specific constants.

A dedicated conversion is faster as it has only one float multiplication runtime.


```cpp
inline float PSI2MSW(float value)
{
  return value * (PSI2MILLIBAR * MILLIBAR2MSW);
}
```

or
```cpp 
#define PSI2MSW     (PSI2MILLIBAR * MILLIBAR2MSW)
...
float out = in * (PSI2MSW);
```



## Operation

```cpp
pressure P;

...

P.setDynes(1000);
Serial.print("mBar: ");
Serial.println(P.getMilliBar()); // 1000 Dynes in mBar
Serial.print("TORR: ");
Serial.println(P.getTORR());     // 1000 Dynes in Torr
```

#### Obsolete

Version 0.1.0 has incorrect setters. fixed in version 0.2.0.


#### Experimental 0.2.1

Apply the ideal gas law : **P x V / n x T = Constant**

- **void change(float T1, float T2, float V1, float V2, float N1, float N2)**
  - T (temperature) in Kelvin,
  - V (volume) in identical units, 
  - N (# atoms) in mole
- **void changeT(float T1, float T2)** only change temperature. T in Kelvin.
- **void changeV(float V1, float V2)** only change volume.
- **void changeN(float N1, float N2)** only change moles.

in code
```cpp
pressure P;    
P.setPressure(...);
P.change(T1, T2, V1, V2, N1, N2);  // apply all changes.  
x = P.getPressure()
```

- do we need a **changeTC(float T1, float T2)** only change temperature, T in Celsius
- should functions return bool true on success ?

Kelvin = Celsius + 273.15;
Kelvin = (Fahrenheit - 32) \* 5 / 9 + 273.15;
Kelvin = Fahrenheit \* 5 / 9 + 290.93;  // one operator less.


## Future

#### must
- update documentation
- find a good reference for conversion formula constants.


#### should
- test with gas law.
- calculate getter constants from setter constants.    1.0 / XXX
- rename parameters so they make more sense?
```
  void  setMilliBar(float milliBar )  { _pressure = milliBar; };
  void  setBar(float Bar)             { _pressure = Bar * BAR2MILLIBAR; };
  void  setPSI(float PSI)             { _pressure = PSI * PSI2MILLIBAR; };
```
- change should return int indicating 
  -  1: a change is made.
  -  0: no change is made
  - -1: parameter negative


#### could
- defaults for functions?  0 like constructor?
- move code to .cpp file ?
