# Table Of Contents
- [Table Of Contents](#table-of-contents)
- [General](#general)
- [Structure](#structure)
    - [Comment](#comment)
        - [Comment String](#comment-string)
            - [Standard Field Names](#standard-field-names)
- [Sources](#sources)

# General
- Used in flac
- `field string` is a case insensitive ascii string with characters in range 0x20 through 0x7D without the character `=` (0x3D)
- `string` is a UTF-8 encoded string
- All numbers are stored in a LE byte order

# Structure
| Size | Index | Name                | Description                                   | Value    |
|------|-------|---------------------|-----------------------------------------------|----------|
| 4    | 0     | Vendor Length       | size of the **Vendor String**                 | unsigned |
| ?    | 4     | Vendor String       | describes the vendor                          | string   |
| 4    | ?     | Comment List Length | size of the **Comment List**                  | unsigned |
| ?    | ?     | Comment List        | **Comment List Length** number of **Comment** |          |
| 1    | ?     | Framing Bit         | if first bit is unset, an error occured       | unsigned |

## Comment
| Size | Index | Name           | Description                  | Value    |
|------|-------|----------------|------------------------------|----------|
| 4    | 0     | Comment Length | Length of **Comment String** | unsigned |
| ?    | 4     | Comment String | A **Comment String**         |          |

### Comment String
| Size | Name        | Description                                  | Value        |
|------|-------------|----------------------------------------------|--------------|
| ?    | Field Name  | name of the field                            | field string |
| 1    | Separator   | separates **Field Name** and **Field Value** | "="          |
| ?    | Field Value | value of the field                           | string       |

- Multiple comments with the same **Field Name** may appear

#### Standard Field Names
| Name         | Description                                                               |
|--------------|---------------------------------------------------------------------------|
| TITLE        | track/work name                                                           |
| VERSION      | differentiates different multiple versions of the same track (e.g. remix) |
| ALBUM        | collection name to which this track belongs                               |
| TRACKNUMBER  | number of track in the collection                                         |
| ARTIST       | the artist generraly considered responsible for the work                  |
| PERFORMER    | the artist who performed the work                                         |
| COPYRIGHT    | copyright attribution                                                     |
| LICENSE      | license information                                                       |
| ORGANIZATION | name of the organization producing the trag (record label)                |
| DESCRIPTION  | short text description of the contents                                    |
| GENRE        | short text indication of the genre                                        |
| DATE         | date the track was recorded                                               |
| LOCATION     | location where the track was recorded                                     |
| IRSC         | IRSC number for the track                                                 |

- Date can be just a year number

# Sources
- [xiph](https://www.xiph.org/vorbis/doc/v-comment.html)