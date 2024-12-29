# Simple Stringbuilder for Twincat3

## Motivation

The goal was to create a string concatenation tool in TwinCAT 3 that mirrors the ease of use found in object-oriented programming languages like C#:

```csharp
var stringTest = "test";
var i = "1";
var j = "1.2345";

Console.WriteLine($"This is a string: {stringTest}, this is an integer: {i}, this is a float: {j}");
```

- Avoids Global Variables and Constants for config: Instead of relying on global variables or constants for parametrization, this StringBuilder allows for dynamic buffer size configuration through the Init method:

```st
StringBuilder.Init(maxBufferSize:=3000);
```

- Dynamic Configuration: This approach supports dynamic software configuration at machine startup using configuration files (e.g., JSON), steering clear of FB_init methods for broader flexibility.

## Usage

- Method Chaining: You can chain methods to append various data types easily:

```st
StringBuilder
    .Append(Value:='This is a bool: ')
    .AppendAny(Value:=boolExample)
    .Copy(pDest:=ADR(simpleUseOutput2), n:=SIZEOF(simpleUseOutput2));
```

### Without instantiation

Simple function call, which creates an instance at GVL_StringBuilder. This avoids having multiple instantiations of this class in one project.

```st
BuildString(nSizeDest:=SIZEOF(simpleUseOutput1))
			.AppendAny(boolExample)
			.AppendAny(dintExample)
			.Copy(pDest:=ADR(simpleUseOutput1), n:=SIZEOF(simpleUseOutput1));
```

### With instantiation

```st

StringBuilder : CStringBuilder;
stStringBuilderPara : ST_StringBuilderPara := (MaxBufferSize:=3000);
```

```st
StringBuilder.Init(pPara:=ADR(THIS^.stStringBuilderPara))

StringBuilder
    .Append(Value:='This is a bool: ')
    .AppendAny(Value:=boolExample)
    .Append(Value:=', This is a byte: ')
    .AppendAny(Value:=byteExample)
    .Append(Value:=', This is a word: ')
    .AppendAny(Value:=wordExample)
    .Append(Value:=', This is a dword: ')
    .AppendAny(Value:=dwordExample)
    .Append(Value:=', This is a lword: ')
    .AppendAny(Value:=lwordExample)
    .Append(Value:=', This is a sint: ')
    .AppendAny(Value:=sintExample)
    .Append(Value:=', This is an int: ')
    .AppendAny(Value:=intExample);

```

- Output: For testing or logging, you can write the result to a file or copy it to a temporary string:

```st
// display string data by copying buffer to given string
StringBuilder.Copy(pDest:=ADR(simpleUseOutput2), n:=SIZEOF(simpleUseOutput2));
```

### Example Output::

```
'This is a bool: TRUE, This is a byte: 255, This is a word: 65535, This is a dword: 4294967295, This is a lword: 18446744073709551615, This is a sint: -128, This is an int: -32768'
```

This StringBuilder for TwinCAT 3 offers a clean, flexible way to manage string concatenation, similar to more modern programming paradigms, while aligning with the platform's capabilities.
