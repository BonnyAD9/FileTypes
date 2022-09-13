# Table Of Contents
- [Table Of Contents](#table-of-contents)
- [General](#general)
- [Structure](#structure)
    - [List Of Genres](#list-of-genres)
        - [Standard](#standard)
- [Sources](#sources)

# General
- Usually used in mp3 files
- Stored at the end of the file
- non numeric strings are padded either with NULL (`\0`) or spaces (` `)

# Structure
Total size: 128

| Size   | Index | Name      | Description                                                            | Value          |
|--------|-------|-----------|------------------------------------------------------------------------|----------------|
| 3      | 0     | Header    |                                                                        | "TAG"          |
| 30     | 3     | Title     | Track title                                                            | string         |
| 30     | 33    | Artist    | Artist name                                                            | string         |
| 30     | 63    | Album     | Album name                                                             | string         |
| 4      | 93    | Year      | Release year                                                           | numeric string |
| 28, 30 | 97    | Comment   | The comment                                                            | string         |
| 1      | 125   | Zero-Byte | If this is zero, **Comment** has length of 28 and **Track** is present |                |
| 1      | 126   | Track     | Track number                                                           | unsigned       |
| 1      | 127   | Genre     | Index in a **List Of Genres**                                          | unsigned       |

## List Of Genres

### Standard
| Index | Genre             |
|-------|-------------------|
| 0     | Blues             |
| 1     | Classic Rock      |
| 2     | Country           |
| 3     | Dance             |
| 4     | Disco             |
| 5     | Funk              |
| 6     | Grunge            |
| 7     | Hip-Hop           |
| 8     | Jazz              |
| 9     | Metal             |
| 10    | New Age           |
| 11    | Oldies            |
| 12    | Other             |
| 13    | Pop               |
| 14    | Rhythm and Blues  |
| 15    | Rap               |
| 16    | Reggae            |
| 17    | Rock              |
| 18    | Techno            |
| 19    | Industrial        |
| 20    | Alternative       |
| 21    | Ska               |
| 22    | Death Metal       |
| 23    | Pranks            |
| 24    | Soundtrack        |
| 25    | Euro-Techno       |
| 26    | Ambient           |
| 27    | Trip-Hop          |
| 28    | Vocal             |
| 29    | Jazz & Funk       |
| 30    | Fusion            |
| 31    | Trance            |
| 32    | Classical         |
| 33    | Instrumental      |
| 34    | Acid              |
| 35    | House             |
| 36    | Game              |
| 37    | Sound clip        |
| 38    | Gospel            |
| 39    | Noise             |
| 40    | Alternative Rock  |
| 41    | Bass              |
| 42    | Soul              |
| 43    | Punk              |
| 44    | Space             |
| 45    | Meditative        |
| 46    | Instrumental Pop  |
| 47    | Instrumental Rock |
| 48    | Ethnic            |
| 49    | Gothic            |
| 50    | Darkwave          |
| 51    | Techno-Industrial |
| 52    | Electronic        |
| 53    | Pop-Folk          |
| 54    | Eurodance         |
| 55    | Dream             |
| 56    | Southern Rock     |
| 57    | Comedy            |
| 58    | Cult              |
| 59    | Gangsta           |
| 60    | Top 40            |
| 61    | Christian Rap     |
| 62    | Pop/Funk          |
| 63    | Jungle            |
| 64    | Native US         |
| 65    | Cabaret           |
| 66    | New Wave          |
| 67    | Psychedelic       |
| 68    | Rave              |
| 69    | Show tunes        |
| 70    | Trailer           |
| 71    | Lo-Fi             |
| 72    | Tribal            |
| 73    | Acid Punk         |
| 74    | Acid Jazz         |
| 75    | Polka             |
| 76    | Retro             |
| 77    | Musical           |
| 78    | Rock 'n' Roll     |
| 79    | Hard rock         |

# Sources
- [wikipedia](https://en.wikipedia.org/wiki/ID3)