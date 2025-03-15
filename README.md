# Sisop-1-2025-IT40
Submission for the KCKS Lab - Operating System Practicum 2025 (Modul 1)

# SOAL NOMOR 1

# SOAL NOMOR 2

# SOAL NOMOR 3



# SOAL NOMOR 4

## PokeSUSS - Pokémon Usage Analysis Script

## Overview Singkat
PokeSUSS merupakan sebuah script bash yang saya tulis untuk menciptakan program yang bisa menganalisis data Pokemon dari suatu data dalam file CSV (Comma Separated Value) sehingga pekerjaan untuk mencari Pokemon pilihan jadi lebih mudah. Berikut ini merupakan fitur-fitur yang ada dalam program PokeSUSS ini:

- Menampilkan Pokemon dengan penggunaan tertinggi (adjusted dan raw)
- Mengurutkan data berdasarkan kolom tertentu
- Mencari Pokemon berdasarkan nama
- Menampilkan bantuan penggunaan

```bash
#!/bin/bash
# Cek argumen minimal
if [ $# -lt 2 ]; then
 echo "Error: Need more argument boys! use --help for option information."
    exit 1
fi

FILE=$1
COMMAND=$2
OPTION=$3
if [ ! -f "$FILE" ]; then
    echo "Error: File '$FILE' file not found my lord!"
    exit 1
fi

show_info() {
highest_usage=$(awk -F, 'NR > 1 { if ($2+0 > max_usage) { max_usage = $2+0; name=$1 } } END { print name, max_usage"%" }' "$FILE")
highest_raw=$(awk -F, 'NR > 1 { if ($3+0 > max_raw) { max_raw = $3+0; name=$1 } } END { print name, max_raw" uses" }' "$FILE")
    
echo "Summary of $FILE"
echo "Highest Adjusted Usage: $highest_usage"
echo "Highest Raw Usage: $highest_raw"

}
sort_data() { if [ -z "$OPTION" ]; then  echo "Error: Plz spesialize column for sorting!"
exit 1
fi

case "$OPTION" in
 usage) column=2 ;;
 raw) column=3 ;;
 name) column=1 ;;
 hp) column=6 ;;
 atk) column=7 ;;
 def) column=8 ;;
 spatk) column=9 ;;
 spdef) column=10 ;;
 speed) column=11 ;;
 *) echo "Error: column is not valid for sorting!." ; exit 1 ;;
esac
{ head -n 1 "$FILE" awk -F, -v col="$column" 'NR > 1 { print $0 | "sort -t, -k"col" -nr" }' "$FILE"} > sorted_output.csv

cat sorted_output.csv }

search_pokemon() {
	if [ -z "$OPTION" ]; then
         echo "Error: name the pokemon!!!"
exit 1
fi

{ head -n 1 "$FILE"   awk -F, -v name="$OPTION" 'NR > 1 && tolower($1) ~ tolower(name) { print }' "$FILE"} > search_output.csv

cat search_output.csv }

show_help() {
YELLOW='\e[33m'
RED='\e[31m'  # buat merah
BLUE='\e[34m' # ini biru 
NC='\e[0m'    # Reset warna

echo -e "Usage: ./pokemon_analysis.sh <file.csv> <command> [options]"
echo -e "                                    WELCOME TO POKESUSS!!!"
cat suss.txt
echo -e "${YELLOW}Commands:${NC}"
echo -e "  ${RED}--help${NC}           ${BLUE}Display Help${NC}"
echo -e "  ${RED}--info${NC}           ${BLUE}Display highest adjusted and raw usage${NC}"
echo -e "  ${RED}--sort <column>${NC}  ${BLUE}Sort the data by specific column${NC}"
echo -e "     ${RED}name${NC}          ${BLUE}Sort by Pokemons name${NC}"
echo -e "     ${RED}usage${NC}         ${BLUE}Sort by adjusted usage${NC}"
echo -e "     ${RED}raw${NC}           ${BLUE}Sort by raw usage${NC}"
echo -e "     ${RED}hp${NC}            ${BLUE}Sort by HP${NC}"
echo -e "     ${RED}atk${NC}           ${BLUE}Sort by Attack${NC}"
echo -e "     ${RED}def${NC}           ${BLUE}Sort by Defense${NC}"
echo -e "     ${RED}spatk${NC}         ${BLUE}Sort by Special Attack${NC}"
echo -e "     ${RED}spdef${NC}         ${BLUE}Sort by Special Defense${NC}"
echo -e "     ${RED}speed${NC}         ${BLUE}Sort by Speed${NC}"
echo -e "  ${RED}--grep <name>${NC}    ${BLUE}Search Pokemons by name${NC}"
}

case "$COMMAND" in
--help) show_help ;;
--info) show_info ;;
--sort) sort_data ;;
--grep) search_pokemon ;;
--filter) search_pokemon ;;
*) echo "Error: unknown command ma boyss!" ; exit 1 ;;
esac

```

## Usage

### Cara Menjalankan Script
```bash
./pokemon_analysis.sh pokemon_usage.csv <command> [options]
```

- `<pokemon_usage.csv>`: File CSV yang berisi data Pokemon (sudah disediakan).
- `<command>`: Perintah yang ingin dijalankan.
- `[options]`: Opsi tambahan tergantung pada perintah yang digunakan.

### Commands

#### 1. `--help`
Menampilkan panduan penggunaan script.
```bash
./pokemon_analysis.sh pokemon_usage.csv --help
```

#### 2. `--info`
Menampilkan Pokemon dengan penggunaan tertinggi (adjusted & raw).
```bash
./pokemon_analysis.sh pokemon_usage.csv --info
```

#### 3. `--sort <column>`
Mengurutkan data berdasarkan kolom tertentu.
```bash
./pokemon_analysis.sh pokemon_usage.csv --sort <column>
```

**Kolom yang tersedia:**
- `name` (Nama Pokemon)
- `usage` (Adjusted Usage)
- `raw` (Raw Usage)
- `hp` (HP)
- `atk` (Attack)
- `def` (Defense)
- `spatk` (Special Attack)
- `spdef` (Special Defense)
- `speed` (Speed)

#### 4. `--grep <name>`
Mencari Pokemon berdasarkan nama.
```bash
./pokemon_analysis.sh pokemon_usage.csv --grep <name>
```

## Dependencies
Script ini memakai perintah dasar dari Unix/Linux (jika belum punya, install dulu `dos2unix`):

- `awk`
- `sort`
- `cat`

Pastikan sistem operasi yang digunakan memiliki bash dan perintah-perintah tersebut tersedia.

## Error Handling
Beberapa pesan error yang mungkin muncul:

- **Error: Need more argument boys! use --help for option information.**
  - Terjadi jika jumlah argumen yang diberikan kurang.
- **Error: File 'pokemon_usage.csv' file not found my lord!**
  - File CSV yang diberikan tidak ditemukan.
- **Error: Plz spesialize column for sorting!**
  - Opsi kolom untuk sorting tidak diberikan.
- **Error: column is not valid for sorting!**
  - Kolom yang diberikan tidak valid.
- **Error: name the pokemon!!!**
  - Nama Pokemon tidak diberikan dalam pencarian.
- **Error: unknown command ma boyss!**
  - Perintah tidak dikenali oleh script.

## Behind the Development
Pembuatan script ini berawal dari kebutuhan untuk mengolah dan menganalisis data Pokemon dengan cepat tanpa memerlukan software tambahan seperti Excel atau Python (Sebenernya gara-gara Praktikum Sistem Operasi sih :v). Dengan menggunakan Bash dan perintah dasar Unix, script ini dirancang untuk memberikan hasil yang efisien dan mudah digunakan.

Beberapa tantangan yang dihadapi dalam proses pengembangan:

- **Parsing File CSV**: Menggunakan `awk` untuk membaca dan mengolah data dengan format yang bervariasi.
- **Sorting Data**: Memastikan sorting berdasarkan berbagai kolom dapat berjalan dengan baik menggunakan `sort`.
- **Error Handling**: Menambahkan warning untuk memastikan input pengguna sesuai dengan kebutuhan.
- **Interaksi dengan Pengguna**: Menggunakan warna-warna yang keche dalam output untuk memberikan pengalaman yang lebih baik saat menjalankan script di terminal.
- **Best UI-Art**: Menggunakan desain background yang luar biasa hebat dan indah dengan Mas Gundul Suss sebagai Brand Ambassador-nya.

```txt
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░╔═╗░░░░░╔╗░░░░░░░░░░░░░╔═══╦═══╦╗╔═╦═══╗░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░║╔╝░░░░░║║░░░░░░░░░░░░░║╔═╗║╔═╗║║║╔╣╔══╝░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░╔╝╚╦╦═╗╔═╝║╔╗░╔╦══╦╗╔╦═╗║╚═╝║║░║║╚╝╝║╚══╦╗╔╦══╦═╗░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░╚╗╔╬╣╔╗╣╔╗║║║░║║╔╗║║║║╔╝║╔══╣║░║║╔╗║║╔══╣╚╝║╔╗║╔╗╗░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░║║║║║║║╚╝║║╚═╝║╚╝║╚╝║║░║║░░║╚═╝║║║╚╣╚══╣║║║╚╝║║║║░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░╚╝╚╩╝╚╩══╝╚═╗╔╩══╩══╩╝░╚╝░░╚═══╩╝╚═╩═══╩╩╩╩══╩╝╚╝░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░╔═╝║░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░╚══╝░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
																				░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                                                                                                                                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                                                        .*((/(//(((((((((((((((##(*.                                            ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                                                 ,((///////////(((((((((((((((((((((####(,                                      ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                                          *((((/////////////((((((((((((((((((((((((########(.                                  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                                     /(///////*/******//////((((((((//(((((((((################/                                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                                 ,(/////////*********////////(((((//(((((((((((###################                              ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                               *////////***********///////((((((((((((((((((((######################                            ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                             *///////********/**///////**/(///((((((((((((((##########################                          ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                           .///////**/*****/////////*/*////((((((((((((((((#########################%##/                        ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                          *//////***////**///////////////////((((((((((((###########################%%##%.                      ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                         *////////*////////////*////*////////(((((((((((#############%%%%%%#####%%########/                     ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                        /////////////////////////*///////////(((((((((#############%%%%%%%%%%%%%%%##%%%%%###                    ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                      .///***/***///**/**////*////////**////(((((((############%%%%%%%%%%%%%%%%%%%%%%%%%%%%#%                   ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                     ,/******************///**//////******//(((((##((#######%%####%%%%%%%%%%%%%%%%%%%%%%%%%%%*                  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                    ,//*******************//////////******//((((((((((########%%%%###%%%%%%%%%%%%%%%%%%%%%%%%%                  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                   ,//*********************//////**//**///////((((((((((########%%%%%%%##%%%%%%%%%%%%%%%%%%%%%/                 ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                   .*/************************/////////*///////#####(((((((#####%%#%##%%%%%%##%%%%%%%%%%%%%%%%%%                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 
                                                  (/***********************//**///***/(((         %%%#%%%########%%%%%%%%%%%%##%%%%#%%%%%%%%%%%*                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                                 //*********************/******/***//(((            #%%%%%%%%%####%%%%%%%%#####%%%####%%%%%%%%%#                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                       //(/*(#, ,#%/****,***************************//    (((((((      %%%&&&&%%%%%%%%%%%%%%%###%#####%%%%%%%%%%                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                      *##****/(##%(****************************,,***   (((((########     ##%%%&&&%%%%%%%%%%%%%##%%###%%%%%%%%%%%                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                     ,#%%/*,.,/(((//*****************************,*//((##%%%&&%&&&&&&&%##   ((#%%&&&&&%%%%%%%&%%%%%%#%%%%%%%%%%%                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                     /#%&/,,*//,*//******************//(/*****,**/((##%%&%##((((((#%%%&&@@@   %###%&%&%%%%%&&&%%%&%%%%&&%&&&&&%%                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                     /%%(**///(,,**,,****************/*******///((#%%&%#/*/(   0000   %%%&&@@@   &&##%%%&&&&&%###############&&&&               ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  
                                     *#/*//**(/****,,**********************///((##%%%&%##(,   000000   &&&&&@@@@&&%###%%&&%(/(# ==%%%%%%%%%%=&&&%               ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  
                                     .(/####%%%(***,,*********************//(((######%(//(/    000000    ###&@@@@&&%(((##(//(#%&@@  0000 %%%%&&&&               ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░  
                                      /#%%&&&&#****,,**,*,,,***,********////(((######((/,,(     0000     %&@@@&@@&&&%(//////(#%&@@@  00 @@&&&&&&                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 
                                      /##%%%&#**//,,**,,**,,,,,********/////(((((##((((((((((        #&&&&&@@&&&&%##(/////(#%&&@@@@@@@@@@@@@@@@*                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░ 
                                      /**,*/(##,**,***************,**//////(((((((((((((((#####%%%%&&&&&&&&&&%%#((((/**//(#%%@@@@@@@@@@@@&&&&#                  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                      *(,,,*/(%,,,,,,****************////((((((((((((((((((####%%%%%&&&&&%%####((//*****/(###&@@@@@@@@@@@@&@/                   ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                       *,,*,,,,**,,,,,************,***///((((((((((((((((#####%%%%%%%%%#((((((((///****///(##%@@@@@@@@@@@@@                     ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                        *,,,,*/***,**************,,,,,///((((###############%%%%%%##((((((((((((////****//(##%&&&@@@@@@@&&                      ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                         **,,***(/,**********/**,,,,,**/((#######################((((((((((((((////****///((#%&&&&&&&&&&&#                      ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                          .***/(#/************/****,,*//(((############%#############((((/(((//***********(##%%%&&&&&&&&&#                      ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                           .***/*********************//((((##########################(((///***************((##%&%&&&&&&&%%                      ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                           ****/**********************/(((##########(######((#######(((//********,,****/*/((##%%%%&&&&&&&#                      ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                           ****/**************,*****//((((##%#######(((##(((((######(((/****/////*,****///(###%%&&&&&&&&&.                      ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                          ,****//****************,***///((######((((((((((((((#######(///***///((/****////((###%&&&&&&&&*                       ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                          ******/*************,,*,****//(((((##(((((////////((((((##(/**,,,**///*******//(((######&&&&@,                        ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                          ******////***,*,******,*,***////(/(((((((////*///*/////((#(*,,,,,*///////**//((####%%%%%%&@&                          ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                          *******///******,*,,,,******//////**/////***/*/******//((##(**/((#%&&&&#(///((##%&&&&&&&&@.                           ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                          *********/******,,,,,,*****////(********************////(((%%#(%&&&##(((#%(/(##%%&&&&&@&#                             ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                          ********//*******,,*,,,****///(***,*****//,,,,**/**/////(((#%%%%&%%##((#%%#(#%%&&&&&&&&/                              ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                         .**************,,,,,,,,,***////***,**,*//,,,,,*****/////((######%%%&%%%%%&&@@@@@&&&&%&&(                               ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                         /***************,,,,,,,,****///*,,,*****,,,,*****//(((((((######%%%&&&%%%&@@@@@@&&&&%%%                                ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                       ,**********//*****,,,,,,,,***///***,,,,,,,,,,**,**///((((((###%%%%%%%&&&%&&&@@@@&&&&&&&&                                 ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                     ./********///////****,,,,,,**/*/****,,,,,,,,,*****//((###((#(##%##%%%%%%%%%%&&&&&&&&&&&&&,                                 ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                    ****/******/////////**********/***,,,,,,,,,,,*////((#%%%&&%%%%%%%&&___-----------&&@&&&&@.                                  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                  */**********///////////***********/**,,,,,,,,*//(####%%%%%%%&&&&&%##-===============@@@@@&                                    ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                                /***/*********///////////////******/**,*,,*,,**(((######(((((((###%%===%%&&&&&&&&&&&@@@@&&%                                     ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                             */****/*********///*///////////*//*******,,,,****/(######(((/(((####((####%%%&&&&&&&&&&&&&&&&#/.                                   ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                         .//////*///**********//////(/////(///*////*****,*****((#####((///(((##%%%%%%%%%%%&&&&&&&&&&&&&&&&%(%%%%%#,                             ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                     .////**/*/////***********///////(((/////((//////*******//(((###(#((//((##%%%%%%%&&&@@@@@@@@@&&&&&&&%%&&##%#%##%&%(                         ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
                 .*/*/***///*//((/**********//////////((///((((((((((///*****//(((######((###%#####%%%%&&&&&&&&&&&&&&&&&&&&&&%%%%%##%%%%%%(                     ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
             .*/*********/////((//**/*******/////////((//(((((((#####((/////**//(/(#(#(((((((((#######%%%&&&&&&&&&&&&&&&&&&&&@(%%%%##%%%%%%%%%                  ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
         */************///(((##((////*//***//**/*////((((((((########(((((////****/((((((((///((####%%%%%%%&&&&&&&&&&&&&&&&&@@#(&&###%%%%%#%%%%%&*              ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
     */ ,/**********///((((#####(//////////////***////(((((#(####%%##(/((((((///*////(#((#(//(####%%%%%%%&&&&&&&&&&&&&&&&@@@@@%((&%##%%%%&&%%#%%#%%%%%%(,       ░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
.///**** */*******//(((###%%%%##((((((//////**///////((((((#%#%##%%%#(((((###%%#(((///(##((##(####%%%%%%&&&&&&&&&&&&&&&&&&@@@@&(((%%#%%%%%%%%%%%#%%%%%%%%%%%%&%#░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
********* /(/**///((###%%%%%&%%%####(///////**////////((((####%%&&&%%#((####%%%%&@&#((((#####(#%#%%%&&&&&&&&&&&&&&&&&&&&&&@@@@&#(((##%%&%%%%%%%%%%%%%%%%%%%&&%%%░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
*********../((((((###%%%%&&&&&&%%###((////*////////(((((###%%%%%&&&&&%###%%%%&&&@@@@@@&#########%%%&&&&&&&&&&&&&&&&&&&&&&&@@@@&#((((#%%%%%%%%%%%%%%%%%###%%%%%%&░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
**********../#####%%%%%&&&&&&&%%%%#((((///////////((((####%%%%%&&&&&&&&&&%%&&&@@@@@@@@@@@@@&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&@@@&#(((/(##%%%%%%%%%%%%#%%%%%%%%%%%%░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
***/*****// ,/##%%%%%%&&&&%%%%%%%##(((((///////////((####%%%%%&&&&&&@@@@@&&@@@@@@@@@@@@@@&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&@%#(((/(/###%%#%%%%%%%%##%%%%%%%%%%░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
*/*///////(( ,*#%%%%%&&%%%%%%%%%##(/((((((///////((((####%%%%%&&&&&@@@@@@@@@@@@@@@@@@@@@&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&&@%#(((((//###%%%###%%%%###%%%%%%%%%░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░███████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░██░░░░█░░░░░░░░░░██░░░██░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░██░░░██░░░░░░░░░░██░███░░░░█████░░░░██████░░░█░░░░██░░█████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░██████░░░██████░░███░░░░░░██░░░██░░██░░░░░░░░██░░░██░██░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░██░░░░░░██░░░░█░░████░░░░████████░░███████░░░██░░░░█░███░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░██░░░░░░██░░░░█░░██░░██░░██░░░░░░░░░░░░░░██░░██░░░██░░░░████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░██░░░░░░░█████░░░██░░░██░░██████░░████████░░░██████░░███████░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░
░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░


```
## Code Breakdown (Penjelasan Per Baris Kode)

### Shebang
```bash
#!/bin/bash
```
Menentukan script ini agar dieksekusi menggunakan Bash.

### Validasi Jumlah Argumen
```bash
if [ $# -lt 2 ]; then
  echo "Error: Need more argument boys! use --help for option information."
  exit 1
fi
```
- `$#` adalah jumlah argumen yang diberikan ke script.
- `-lt` berarti "less than" (kurang dari), digunakan untuk mengecek apakah jumlah argumen kurang dari 2.
- `exit 1` menghentikan eksekusi script dan mengembalikan kode error 1.

### Menyimpan Argumen
```bash
FILE=$1
COMMAND=$2
OPTION=$3
```
- `$1`, `$2`, `$3` menyimpan nilai argumen pertama, kedua, dan ketiga.

### Validasi File CSV
```bash
if [ ! -f "$FILE" ]; then
  echo "Error: File '$FILE' file not found my lord!"
  exit 1
fi
```
- `-f` mengecek apakah file ada.
- `! -f` mengecek apakah file tidak ada.

### Fungsi `show_info`
```bash
show_info() {
  highest_usage=$(awk -F, 'NR > 1 { if ($2+0 > max_usage) { max_usage = $2+0; name=$1 } } END { print name, max_usage"%" }' "$FILE")
  highest_raw=$(awk -F, 'NR > 1 { if ($3+0 > max_raw) { max_raw = $3+0; name=$1 } } END { print name, max_raw" uses" }' "$FILE")
  echo "Summary of $FILE"
  echo "Highest Adjusted Usage: $highest_usage"
  echo "Highest Raw Usage: $highest_raw"
}
```
- `awk -F,` digunakan untuk memisahkan kolom berdasarkan koma.
- `NR > 1` memastikan baris pertama (header) diskip.

### Fungsi `sort_data`
```bash
sort_data() {
  if [ -z "$OPTION" ]; then
    echo "Error: Plz spesialize column for sorting!"
    exit 1
  fi
```
- `-z` mengecek apakah variabel kosong.

```bash
  case "$OPTION" in
    usage) column=2 ;;
    raw) column=3 ;;
    name) column=1 ;;
    hp) column=6 ;;
    atk) column=7 ;;
    def) column=8 ;;
    spatk) column=9 ;;
    spdef) column=10 ;;
    speed) column=11 ;;
    *) echo "Error: column is not valid for sorting!." ; exit 1 ;;
  esac
}
```
- `case ... esac` itu kayak `switch-case` di bahasa yang lain.

### Fungsi `search_pokemon`
```bash
search_pokemon() {
  if [ -z "$OPTION" ]; then
    echo "Error: name the pokemon!!!"
    exit 1
  fi
```
- `tolower()` mengubah teks menjadi huruf kecil agar pencarian tidak case-sensitive.


# Dokumentasi Erorrrrrrrrrrrrrr
#
# typo spaci

![tolol](https://github.com/user-attachments/assets/53262fff-9758-4d57-9337-91c5fdb03b08)

# 

# Pemberian access yg gagal terus (jadinya pake unix ajjah)
![permission denied](https://github.com/user-attachments/assets/54e3862a-b316-4e2c-8284-27b6943f83aa)
![permission Give Acces](https://github.com/user-attachments/assets/57eaaa1e-9819-4d62-a1d9-a5d6c7f87aaa)
![dos2unix](https://github.com/user-attachments/assets/906ef00d-17ae-45db-8c1b-046eb1257f94)

# 

# Perbaikan eror line 87 (lupa bracket penutup untuk fungsi show_info() )
![lupa bracket](https://github.com/user-attachments/assets/99768835-b503-4939-b599-021a135b8c19)
![fix -lupa bracket](https://github.com/user-attachments/assets/a8ee1421-f160-4d65-9ced-3a9c64c3c86c)

# 

# Walawe namanya malah kebalikk
![--sort name - Terbalik](https://github.com/user-attachments/assets/35d7a0e7-7011-4383-986e-10f3ff250172)
![--sort name - Before](https://github.com/user-attachments/assets/20dd3dc0-060e-424d-b26f-b63e9f7247ec)
![--sort name - After](https://github.com/user-attachments/assets/d52e71b1-072b-4d76-9610-a45347a260e0)









## Author
5027241024_Ahmad Ibnu Athaillah
