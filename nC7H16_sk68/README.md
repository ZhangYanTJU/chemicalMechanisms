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

# fuelName
NC7H16

# OpenFOAM
```
chemkinToFoam chem.inp therm.dat ../transportProperties nC7H16_sk68.OFchem nC7H16_sk68.OFtherm
```
You will get `nC7H16_sk68.OFchem` and `nC7H16_sk68.OFtherm`

# FlameMaster
```
ScanMan -i chem.inp -t therm.dat -m tran.dat -f chemkin -o nC7H16_sk68.pre
rm chem.inp.h chem.inp.chmech chem.inp.chthermo chem.inp.chtrans
```
You will get `nC7H16_sk68.pre`

# Cantera
```
python -m cantera.ck2cti --input=chem.inp --thermo=therm.dat --transport=tran.dat --output=nC7H16_sk68.cti
```
You will get `nC7H16_sk68.cti`

# Source

https://www.sciencedirect.com/science/article/pii/S0010218009000789#aep-bibliography-id31