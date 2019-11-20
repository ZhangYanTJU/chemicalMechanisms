# pre-processing

```
dos2unix chem.inp
dos2unix thermo.dat
dos2unix trans.dat
sed -i '/^$/d' chem.inp # remove blank lines
sed -i '/^$/d' thermo.dat # remove blank lines
sed -i '/^$/d' trans.dat # remove blank lines
sed -i 's/[a-z]/\u&/g' chem.inp #Captical
sed -i 's/[a-z]/\u&/g' thermo.dat #Captical
sed -i 's/[a-z]/\u&/g' trans.dat #Captical
echo "" >> chem.inp # add a blank line at the end of chem.inp
sed -i 1,2d thermo.dat # delete the first 2 lines
sed -i '1i \ \ \ 200.000  1000.000  5000.000' thermo.dat # add a line
sed -i '1i THERMO ALL' thermo.dat # add a line
sed -r -i "s/CH2\(S\)/CH2-S /g" chem.inp # CH2(S) --> CH2-S
sed -r -i "s/CH2\(S\)/CH2-S /g" thermo.dat # CH2(S) --> CH2-S
sed -r -i "s/CH2\(S\)/CH2-S /g" trans.dat # CH2(S) --> CH2-S
sed -i 's/./ /79' thermo.dat # let $79 column be a space
sed -i "s/NC7H16/C7H16 /g" chem.inp # NC7H16 --> C7H16
sed -i "s/NC7H16/C7H16 /g" thermo.dat # NC7H16 --> C7H16
sed -i "s/NC7H16/C7H16 /g" trans.dat # NC7H16 --> C7H16
sed -i "s/NC12H26/C12H26 /g" chem.inp # NC12H26 --> C12H26
sed -i "s/NC12H26/C12H26 /g" thermo.dat # NC12H26 --> C12H26
sed -i "s/NC12H26/C12H26 /g" trans.dat # NC12H26 --> C12H26
```

# OpenFOAM

```
chemkinToFoam chem.inp thermo.dat ../transportProperties $(basename "$PWD").OFchem $(basename "$PWD").OFtherm
```

In thermophysicalProperties:

```
chemistryReader foamChemistryReader;
foamChemistryFile "***.OFchem";
foamChemistryThermoFile "***.OFthermo";
```

# FlameMaster
```
ScanMan -i chem.inp -t thermo.dat -m trans.dat -f chemkin -o $(basename "$PWD").pre
rm chem.inp.h chem.inp.chmech chem.inp.chthermo chem.inp.chtrans
```


# Cantera
```
ck2cti --input=chem.inp --thermo=thermo.dat --transport=trans.dat --output=$(basename "$PWD").cti
```
