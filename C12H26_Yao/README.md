# Intro
| fuelName      | C12H26 |
| --------------------          | ------------------------------------------------- |
| species       | 54       |
| reactions     | 269        |



# pre-processing
```
dos2unix chem.inp
dos2unix therm.dat
dos2unix tran.dat
sed -i '/^$/d' chem.inp # remove blank lines
sed -i '/^$/d' therm.dat # remove blank lines
sed -i '/^$/d' tran.dat # remove blank lines
sed -i 's/[a-z]/\u&/g' chem.inp #Captical
sed -i 's/[a-z]/\u&/g' therm.dat #Captical
sed -i 's/[a-z]/\u&/g' tran.dat #Captical
echo "" >> chem.inp # add a blank line at the end of chem.inp
sed -i 1,2d therm.dat # delete the first 2 lines
sed -i '1i \ \ \ 200.000  1000.000  5000.000' therm.dat # add a line
sed -i '1i THERMO ALL' therm.dat # add a line
sed -r -i "s/CH2\(S\)/CH2-S /g" chem.inp # CH2(S) --> CH2-S
sed -r -i "s/CH2\(S\)/CH2-S /g" therm.dat # CH2(S) --> CH2-S
sed -r -i "s/CH2\(S\)/CH2-S /g" tran.dat # CH2(S) --> CH2-S
sed -i 's/./ /79' therm.dat # let $79 column be a space
```

# OpenFOAM
```
chemkinToFoam chem.inp therm.dat ../transportProperties C12H26_Yao.OFchem C12H26_Yao.OFtherm
```
You will get `C12H26_Yao.OFchem` and `C12H26_Yao.OFtherm`.
In thermophysicalProperties:
```
chemistryReader foamChemistryReader;
foamChemistryFile "C12H26_Yao.OFchem";
foamChemistryThermoFile "C12H26_Yao.OFthermo";
```

# FlameMaster
```
ScanMan -i chem.inp -t therm.dat -m tran.dat -f chemkin -o C12H26_Yao.pre -N 0.05
rm chem.inp.h chem.inp.chmech chem.inp.chthermo chem.inp.chtrans
```
You will get `C12H26_Yao.pre`

# Cantera
```
python -m cantera.ck2cti --input=chem.inp --thermo=therm.dat --transport=tran.dat --output=C12H26_Yao.cti
```
You will get `C12H26_Yao.cti`

# Source

https://www.sciencedirect.com/science/article/pii/S0010218009000789#aep-bibliography-id31