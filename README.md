# .NET-JSON-Transformer
A Visual Basic .NET implementation that converts a JSON literal into a .NET XDocument

To use the file simply add it to your project and call the JSON.Parse method.

## Syntax
`Public Function Parse(ByVal source As String) As XDocument`

**Parameters**
- *source*
  - Type: System.String
  - The JSON literal to be converted.

- Return Value
  - Type: System.Xml.Linq.XDocument
  - An XML representation of the JSON literal. If the JSON is not parsable, then the method returns Nothing.
  
## Remarks
  * The parser ignores whitespace. So if the JSON literal is:
    ``` json
    {
      "string_key": [
        1,
        2,
        {
          "nested": true
        }
      ]
    }
    ```
  Then it gets parsed as:
  ``` json
  {"string_key":[1,2,{"nested":true}]}
  ```
  
  * The returned XML is a 1-to-1 translation from the JSON. Using the same example as above, the resulting XML would be:
    ```
    <object>
      <array name="string_key">
        <number>1</number>
        <number>2</number>
        <object>
          <boolean name="nested">true</boolean>
        </object>
      </array>
    </object>
    ```

  * The parser does not parse to the exact specifications of the EBNF found on http://www.json.org/ the following list the deviations in this parser:
    * Number: Checks if the number starts with either a positive sign or negative sign
    * Boolean: Checks for "true" or "false" based on case-insensitivity
    * Null: Checks for "null" based on case-insensitivity
    * String: Does not check for "\u" followed by 4 hexadecimal characters as an escape character

## Examples and Demo:
  The following example demonstrates the Parse method.
  
  ``` vb.net
  Option Strict On
  Public Module Module1
	  Public Sub Main()
		  Dim jsonLiteral As String = "{""key"": [1, 2, 3], ""nested"": {""object"": true}}"
		  Console.WriteLine(JSON.Parse(jsonLiteral))
	  End Sub
  End Module
  ```
Fiddle: https://dotnetfiddle.net/G707RC
