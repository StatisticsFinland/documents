# PX-file keyword specification 

This document describe the use of keywords in px-files in Statistics Finland and is therefore not a general standard or recommendation for the use uf px-files.
This document assumes that the px-file follows the syntax specification defined [here](https://github.com/PxTools/Px.Utils/blob/dev/docs/PXFILE_SPECIFICATION.md). If this document conflicts with the syntax spesification, this ducument overrides the other. Note that the linked specification also contains recommendation about the file contents which should be ignored.

The keywords must be written to the px-file in the same order as they appear in this document for legacy reasons.

## Mandatory keywords
Keywords marked with * are conditional, see the description of each keyword for more information.

### CHARSET
```String``` Always use unicode, value "Unicode".

```
CHARSET="Unicode";
```

### AXIS-VERSION
```String``` This spesification is mostly compatible with "2013" spesification for PxWeb, but due to TIMEVAL changes, new version is required.

```
AXIS-VERSION="2024";
```

### CODEPAGE
```String``` Use utf-8 encoding when writing the file. Value "utf-8".

```
CODEPAGE="utf-8";
```

### LANGUAGE
```String``` The default language of the file.

```
LANGUAGE="fi";
```

### LANGUAGES*
```String list``` Use when the table has more than one language.
Multilingual keywords must be defined for each language in this list.

```
LANGUAGES="fi","sv","en";
```

### CREATION-DATE
```String``` The creation time of the table.

```
CREATION-DATE="20201119 09:00";
```

### NEXT-UPDATE
```String``` The planned next update time of the table.

```
NEXT-UPDATE="20250620 08:00";
```

### TABLEID
```String``` Unique identifier for the table. Stays the same between table updates.

```
TABLEID="statfin_abcd_pxt_12ab";
```

### MATRIX
```String``` Unique identifier for this table version.

```
MATRIX="001_12ab_2023";
```

### DECIMALS
```Number``` Default accuracy for the data.

```
DECIMALS=0;
```

### SUBJECT-CODE
```String``` Unique not language dependant code of the subject area.

```
SUBJECT-CODE="ABCD";
```

### SUBJECT-AREA
```String``` ```Language dependant``` Translated names of the subject area.

```
SUBJECT-AREA="abcd";
SUBJECT-AREA[sv]="abcd";
SUBJECT-AREA[en]="abcd";
```

### DESCRIPTION
```String``` ```Language dependant``` Short discription about the tables content.
Used for examaple in table listings, differentiates it from the other tables in the same group.

```
DESCRIPTION="12yb -- Yksityisen kulutuksen kokonaishintatasoindeksi (EU27_2020=100), 2019-2023";
DESCRIPTION[sv]="12yb -- Den totala prisnivån för privat konsumtion (EU27_2020=100), 2019-2023";
DESCRIPTION[en]="12yb -- Price level index for household final consumption expenditure (EU27_2020=100), 2019-2023";
```

### TITLE
```String``` ```Language dependant``` Used to build headers when a selection is made from this table in PxWeb.

```
TITLE="Yksityisen kulutuksen kokonaishintatasoindeksi (EU27_2020=100)";
TITLE[sv]="Den totala prisnivån för privat konsumtion (EU27_2020=100)";
TITLE[en]="Price level index for household final consumption expenditure (EU27_2020=100)";
```
### CONTENTS
```String``` ```Language dependant``` Short discription about the tables content. Used in header building.

```
CONTENTS="Yksityisen kulutuksen kokonaishintatasoindeksi (EU27_2020=100)";
CONTENTS[sv]="Den totala prisnivån för privat konsumtion (EU27_2020=100)";
CONTENTS[en]="Price level index for household final consumption expenditure (EU27_2020=100)";
```

### UNITS
```String``` ```Language dependant``` Table levels units. This is required by PxWeb, functionality unknown. 
Note that the same keyword is also used to define the content dimension value level units (described later in this document). 

```
UNITS="Euroa, Lukumäärä";
UNITS[sv]="Euro, Antal";
UNITS[en]="Euros, Number";
```

### STUB*
```String list``` ```Language dependant``` Names of the row dimensions. Can be omitted if all dimensions are on columns.

```
STUB="Vuosineljännes","Alue","Huoneluku";
STUB[sv]="Kvartal","Område","Antal rum";
STUB[en]="Quarter","Region","Number of rooms";
```

### HEADING*
```String list``` ```Language dependant``` Names of the column dimensions. Can be omitted if all dimensions are on rows.

```
HEADING="Tiedot";
HEADING[sv]="Uppgifter";
HEADING[en]="Information";
```

### CONTVARIABLE
```String``` ```Language dependant``` Name of the content dimension.

```
CONTVARIABLE="Tiedot";
CONTVARIABLE[sv]="Uppgifter";
CONTVARIABLE[en]="Information";
```

### VALUES
```String list``` ```Language dependant``` Names of the values of a dimensions.
The spesifier describes to which dimension the value names belong to.
PxWeb requires that the VALUE entries are in the same order as the dimension names are in STUB and HEADING lists (in that order).

```
VALUES("Vuosineljännes")="2018Q1","2018Q2","2018Q3","2018Q4";
VALUES[sv]("Kvartal")="2018Q1","2018Q2","2018Q3","2018Q4";
VALUES[en]("Quarter")="2018Q1","2018Q2","2018Q3","2018Q4";
```

### TIMEVAL (Time dimension)
```Custom``` ```Language dependant``` Defines the time series and its interval. Each table must have exactly one time dimension.
TIMEVAL has two different formats, a longer list format and a shorter range format.

#### If the time series has a regular interval of one supported unit of time
Use the sorter range format.
```
TIMEVAL("Vuosineljännes")=TLIST(Q1, "20181-20184);
TIMEVAL[sv]("Kvartal")=TLIST(Q1, 20181-20184);
TIMEVAL[en]("Quarter")=TLIST(Q1, 20181-20184);
```

#### If the series has a regular interval which is NOT one unit of time
Use the longer list format that contain timecodes for all of the values. Example: if the series contain every fourth year, use year as the unit.

Important! This is not compatible with the 2013 standard!
```
TIMEVAL("Vuosineljännes")=TLIST(Q1),"20181","20191","20201","20211","20221", "20231";
TIMEVAL[sv]("Kvartal")=TLIST(Q1),"20181","20191","20201","20211","20221", "20231";
TIMEVAL[en]("Quarter")=TLIST(Q1),"20181","20191","20201","20211","20221", "20231";
```

#### Irregular series
If the time series is irregular, all of the values of the series must have the same unit of time. for example series with both year and month values is not valid.

If the series is irregular, use the list form of TIMEVAL and list the timecodes of all values. Use the longest unit of time possible.

Important! This is not compatible with the 2013 standard!

```
TIMEVAL("Vuosineljännes")=TLIST(Q1),"20181","20191","20201","20211","20212","20213","20214","20221","20222","20223","20224","20231","20232","20233","20234";
TIMEVAL[sv]("Kvartal")=TLIST(Q1),"20181","20191","20201","20211","20212","20213","20214","20221","20222","20223","20224","20231","20232","20233","20234";
TIMEVAL[en]("Quarter")=TLIST(Q1),"20181","20191","20201","20211","20212","20213","20214","20221","20222","20223","20224","20231","20232","20233","20234";
```

### CODES
```String list``` ```Language dependant``` List the unique identifiers for the dimension values. PxWeb requires that these are defined for each language and in the same order as the value names (VALUES keyword).

```
CODES("Vuosineljännes")="2018Q1","2018Q2","2018Q3","2018Q4";
CODES[sv]("Kvartal")="2018Q1","2018Q2","2018Q3","2018Q4";
CODES[en]("Quarter")="2018Q1","2018Q2","2018Q3","2018Q4";
```

### VARIABLE-TYPE
```String``` ```Language dependant``` Defines the type of a dimension. Should be defined for each dimension.

Allowed values: ```Content```, ```Time```, ```Geographical```, ```Ordinal```, ```Nominal```, ```Balance```, ```Classificatory```, ```Unknown```

```
VARIABLE-TYPE("Vuosineljännes")="Time";
VARIABLE-TYPE[sv]("Kvartal")="Time";
VARIABLE-TYPE[en]("Quarter")="Time";
```

### MAP*
```String``` ```Language dependant``` Intended to link an area variable to a map data for visualizations. Also sets the variable type to ```Geographical```.

```
MAP("Alue")="Alue 2018";
MAP[sv]("Område")="Alue 2018";
MAP[en]("Region")="Alue 2018";
```

### PRECISION (Dimension value)
```Number``` ```Language dependant``` Sets the precision for the datapoints defined by this dimension value.
This keyword uses the two part specifier because it can be set for any dimension values, not just content dimension.
**IMPORTANT: Only use the precision for content dimension values**

```
PRECISION("Tiedot","Yksityisen kulutuksen kokonaishintatasoindeksi (EU27_2020=100)")=0;
PRECISION[sv]("Uppgifter","Den totala prisnivån för privat konsumtion (EU27_2020=100)")=0;
PRECISION[en]("Information","Price level index for household final consumption expenditure (EU27_2020=100)")=0;
```

### LAST-UPDATED (Content dimension value)
```String``` ```Language dependant``` The last update time of the data described by the value.

```
LAST-UPDATED("Indeksipisteluku")="20240115 08:00";
LAST-UPDATED[sv]("Index tal")="20240115 08:00";
LAST-UPDATED[en]("Point figure")="20240115 08:00";
```

### UNITS (Content dimension value)
```String``` ```Language dependant``` The name of the unit of the data described by the value.

```
UNITS("Alakvartiili")="Euroa";
UNITS[sv]("Undre kvartil")="Euro";
UNITS[en]("Lower quartile")="Euros";
```

### CONTACT (Content dimension value)
```String``` ```Language dependant``` Add contact information 

```
CONTACT("Indeksipisteluku")="<A HREF='https://stat.fi/tilasto/abc' TARGET=_blank>Tilaston kotisivu</A>";
CONTACT[sv]("Index tal")="<A HREF='https://stat.fi/sv/statistik/abc' TARGET=_blank>Statistikens webbsidor</A>";
CONTACT[en]("Point figure")="<A HREF='https://stat.fi/en/statistics/abc' TARGET=_blank>Statistics' homepage</A>";
```

### SOURCE
```String``` ```Language dependant``` Source of data in this table.
PxWeb does not support setting this in content dimension value level.

```
SOURCE="Tilastokeskus, kuluttajahintaindeksi";
SOURCE[sv]="Statistikcentralen, konsumentprisindex";
SOURCE[en]="Statistics Finland, consumer price index";
```

### VALUE-SOURCE (Content dimension value)
```String``` ```Language dependant``` The Source of data defined by a dimension value.
This is Statistics Finlands addition to the format, meant to be used with Px.Utils based systems.

```
VALUE-SOURCE("Indeksipisteluku")="Tilastokeskus, kuluttajahintaindeksi";
VALUE-SOURCE[sv]("Index tal")="Statistikcentralen, konsumentprisindex";
VALUE-SOURCE[en]("Point figure")="Statistics Finland, consumer price index";
```

### OFFICIAL-STATISTICS
```Boolean``` True if this table is part of official statistics.

```
OFFICIAL-STATISTICS=YES;
```

## Optional keywords

### SHOWDECIMAL
```Number``` How many decimals are shown to the end user by default.

```
SHOWDECIMALS=0;
```

### MADE-WITH
```String``` Production system identifier.

```
MADE-WITH="PxBuild";
```

### COPYRIGHT
```Boolean``` Does the table have a copyright. If this keywords is not defined, PxWeb defaults to "NO".

```
COPYRIGHT=YES;
```

### NOTE
```String``` ```Language dependant``` Table or dimension level notes. Can be defined for multiple dimensions.

```
NOTE="<A HREF='https://stat.fi/tilasto/dokumentaatio/abc' TARGET=_blank>Tilaston dokumentaatio</A>";
NOTE[sv]="<A HREF='https://stat.fi/sv/statistik/dokumentation/abc' TARGET=_blank>Dokumentation för statistiken</A>";
NOTE[en]="<A HREF='https://stat.fi/en/statistics/documentation/abc' TARGET=_blank>Documentation of statistics</A>";
```

```
NOTE("Tontin omistusmuoto")="Sijaitseeko asunto omalla vai vuokratontilla.";
NOTE[sv]("Ägarform för tomten")="Är bostaden belägen på en egen tomt eller på en hyrestomt.";
NOTE[en]("Form of ownership of plot")="Is the dwelling located on an own or rented plot.";
```

### NOTEX
```String``` ```Language dependant``` Critical table level notes. Information that the user must know about the table.

```
NOTEX="Sijaitseeko asunto omalla vai vuokratontilla.";
NOTEX[sv]="Är bostaden belägen på en egen tomt eller på en hyrestomt.";
NOTEX[en]="Is the dwelling located on an own or rented plot.";
```

### VALUENOTE
```String``` ```Language dependant``` Dimension value level notes. Can be defined for multiple values.

```
VALUENOTE("Tiedot","Indeksipisteluku")="Pisteluku on hintaindekseissä käytetty muutossuure.";
VALUENOTE[sv]("Uppgifter","Index tal")="Indextalet är en storhet som används inom prisindexen.";
VALUENOTE[en]("Information","Point figure")="Point figure is a change quantity used in price indices.";
```