## Using Plugins to Decompiling and Importing Scripts for Any Game

The plugins are written in non-standard Python with syntax similar to Python 3.4, lacking many built-in libraries and some features, but basic usage is functional.

### Writing Rules
You can refer to [LOOPERS.py](data/LOOPERS.py) and [SP.py](data/SP.py) for examples.

#### Parsing Operation Functions
Define functions in the script with the same name as the OPCODE (case-sensitive), and you can use the script's functions to parse the corresponding operation parameters during decompilation and import. 

#### Init Function
The `Init` function defined in the script is executed once at the very beginning, typically used to call `set_config` and can also perform other initialization operations. 

#### Global Variable opcode_dict
When OPCODE is not loaded or there are unknown OPCODEs, decompilation will output operation names as hexadecimal markers, such as `0x22`, `0x3A`, etc. In this case, you can manually map hexadecimal markers to operation function names using `opcode_dict` as shown below: 
```python
opcode_dict = {
    '0x22': 'MESSAGE',
    '0x25': 'SELECT'
}
```
This means that for an unknown `0x22` operation, the `MESSAGE` function in the plugin will be used for parsing.

### Core Package Built-in Constants
```python
core.Charset_UTF8 = "UTF-8"
core.Charset_Unicode = "UTF-16LE"
core.Charset_SJIS = "Shift_JIS"
```
The characteristics of the three character sets are as follows:
#### Charset_UTF8
1 to 3 bytes per character, with a fixed ending of 0x00.
This encoding is typically used for English strings, and currently, newer games like LOOPERS also use it for expressions (expr) and PAK file names.

#### Charset_Unicode
2 bytes (one uint16) per character, with a fixed ending of 0x0000.
Currently, the majority of Chinese and Japanese text is encoded in this format.

#### Charset_SJIS
1 to 2 bytes per character, with a fixed ending of 0x00.
This encoding is used for expressions and PAK file encoding in older games, typically in single-language Japanese games.

### Core Package Built-in Functions

#### set_config
`set_config(expr_charset, text_charset, default_export=True)`  
Setting Default Values
After setting `expr_charset`, you can obtain the corresponding value through `core.expr`, which is typically consistent with the file name encoding of PAK files.
After setting `text_charset`, you can obtain the corresponding value through `core.text`, and it will also be the default character set value for `read_str` and `read_len_str`.
`default_export` determines whether to export automatically parsed uint16 parameters when an undefined OPCODE is encountered during parsing by the plugin.

#### read
`read(export=False) -> list(int)`
Read `all` parameters in uint16 format; if only one is remaining, it will be read in uint8 format.
#### read_uint8
`read_uint8(export=False) -> int`  
Read one parameter in uint8 format.

#### read_uint16
`read_uint16(export=False) -> int`  
Read one parameter in uint16 format.

#### read_uint32
`read_uint32(export=False) -> int`  
Read one parameter in uint32 format.

#### read_str
`read_str(charset=textCharset, export=True) -> str`  
Read a string with the specified encoding.
Note: If you are certain that a `uint16` indicating the string's length precedes the string, you should use `read_len_str` for reading. Otherwise, incorrect length can lead to import errors.

#### read_len_str
`read_len_str(charset=textCharset, export=True) -> str`  
Read a string with specified encoding that includes its length; during import, the new length will be automatically calculated.

#### read_jump (important)
`read_jump(file='', export=True) -> int`  
Read a jump position parameter in uint32 format; during import, the jump position will be automatically reconstructed.
For cross-file jumps, you need to pass the `file` parameter, which is generally the previous parameter. Cross-file jump is marked as `global233`.
For the `file` parameter in cross-file jumps, you should use read_str or read_len_str to read it, with the character set being expr, typically consistent with the PAK file name encoding.
For in-file jumps, the marker is `label66`.

#### end (important)
`end()`  
Submit the reading that has already been done; this must be called, otherwise, the import and export won't work properly.

#### can_read
`can_read() -> bool`  
Determine whether it's possible to proceed with the next read.

