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
echo "" >> chem.inp # add a blank line at the end of chem.inp, to make FlameMaster happy
sed -i 1,2d thermo.dat # delete the first 2 lines, then we add two lines as follows
sed -i '1i \ \ \ 200.000  1000.000  5000.000' thermo.dat # add a line
sed -i '1i THERMO ALL' thermo.dat # add a line
sed -i 's/./ /79' thermo.dat # let $79 column be a space
sed -r -i "s/CH2\(S\)/CH2-S /g" chem.inp # CH2(S) --> CH2-S
sed -r -i "s/CH2\(S\)/CH2-S /g" thermo.dat # CH2(S) --> CH2-S
sed -r -i "s/CH2\(S\)/CH2-S /g" trans.dat # CH2(S) --> CH2-S
sed -r -i "s/CH2\*/CH2-/g" chem.inp # CH2* --> CH2-
sed -r -i "s/CH2\*/CH2-/g" thermo.dat # CH2* --> CH2-
sed -r -i "s/CH2\*/CH2-/g" trans.dat # CH2* --> CH2-
sed -i "s/NC7H16/C7H16 /g" chem.inp # NC7H16 --> C7H16
sed -i "s/NC7H16/C7H16 /g" thermo.dat # NC7H16 --> C7H16
sed -i "s/NC7H16/C7H16 /g" trans.dat # NC7H16 --> C7H16
sed -i "s/NC12H26/C12H26 /g" chem.inp # NC12H26 --> C12H26
sed -i "s/NC12H26/C12H26 /g" thermo.dat # NC12H26 --> C12H26
sed -i "s/NC12H26/C12H26 /g" trans.dat # NC12H26 --> C12H26
```

# OpenFOAM

At first, check the first species in `chem.inp`, search it in `thermo.data`, get its Tcommon, i.e. the middle temperature for the temperature range.
If the Tcommon of the first species is not 1000 K, then replace it with any species whose Tcommon is 1000K.

```
chemkinToFoam chem.inp thermo.dat ../transportProperties $(basename "$PWD").OFchem $(basename "$PWD").OFthermo
```

In thermophysicalProperties:

```
chemistryReader foamChemistryReader;
foamChemistryFile "***.OFchem";
foamChemistryThermoFile "***.OFthermo";
```

# FlameMaster-1

```
ScanMan -i chem.inp -t thermo.dat -m trans.dat -f chemkin -o $FM_DATA/$(basename "$PWD").pre
rm chem.inp.h chem.inp.chmech chem.inp.chthermo chem.inp.chtrans
```

## FlameMaster-2

```
$FM_BIN/ckintrp3seiser
```

```
thermo.dat
chem.inp
a.i
linkfile
```

```
$FM_PATH/Repository/examples/ScanMan/deprecated/Convert_Mechanisms/mechi2tex.perl a.i 1 2
$FM_PATH/Repository/examples/ScanMan/deprecated/Convert_Mechanisms/modmech.perl -t thermo.dat -r trans.dat -o chem.mech a.mech
CreateBinFile -i newthermofile -o thermo.bin
ScanMan -i chem.mech -t thermo.bin -o $FM_DATA/$(basename "$PWD").pre > chem.out
```

```
rm a.mech a.i a.tex thermo.bin linkfile intmechfile newthermofile chem.out chem.mech chem.h  BroadeningFactors.cp BroadeningFactors.h speciestranslated
```


# PDRs

run as FlameMaster-2, then we got: chem.chmech chem.chthermo chem.chtrans

```
$FM_BIN/ckintrp3seiser
```

```
chem.chthermo
chem.chmech
a.i
linkfile
```

```
$FM_PATH/Repository/examples/ScanMan/deprecated/Convert_Mechanisms/mechi2tex.perl a.i 1 2
```

```
rm a.i a.tex linkfile
```

# Cantera
```
ck2cti --input=chem.inp --thermo=thermo.dat --transport=trans.dat --output=$(basename "$PWD").cti
```
