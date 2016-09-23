# bumper
A/B Testing debug helper framework for iOS

## Example

To run the example project, clone the repo, and run `pod install` from the Example directory first.


## Installation

bumper is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod "bumper"
```

### Input json 
This pod creates all A/B Features from an input json file that consists in an array of Features:

```json
[
	{
		"name": "NameOfFirstFeature",
		"values": ["FirstValue", "SecondValue", "ThirdValue"],
		"description": "First test description"
	},
	{
		"name": "NameOfSecondFeature",
		"values": ["Yes", "No"],
		"description": "Boolean test description"
	},
    {
		"name": "NameOfThirdFeature",
		"values": ["true", "false"],
		"description": "Boolean test description"
	}
]
```
It will generate Enums or Booleans depending on the values you put on the Feature. If a feature consists on two elements and contains positive ("Yes" or "yes" or "true") and negative ("No", "no", "false") values, it will generate a Boolean. The default value for booleans or enums will be the **first** element of the values array

so 
```json
{
"name": "NameOfFirstFeature",
"values": ["FirstValue", "SecondValue", "ThirdValue"],
"description": "First test description"
}
```
will generate:
```swift
// NameOfFirstFeature is an enum with cases: FirstValue, SecondValue, ThirdValue
var nameOfFirstFeature: NameOfFirstFeature 
```

and
```json
{
	"name": "NameOfSecondFeature",
	"values": ["Yes", "No"],
	"description": "Boolean test description"
}
```
will generate:
```swift
var nameOfSecondFeature: Bool
```

### Script
The pod contains a script (scripts/flags_generator/flags_generator.rb) that takes the input json path and an output dir (where the generated swift file will go):
```bash
#> ruby {path_to_project}/scripts/flags_generator/flags_generator.rb -s {path_to_project}/subpath/to/json/file.json -d {path_to_project}/destination/file/dir/
```

### Usage on project

Add the generated `BumperFeatures.swift` file to your project. Then just add the following line on your AppDeleagate previous to any call to any flag:
```swift
Bumper.initialize()
```

and then in any part of your code you can call:
```swift
switch Bumper.nameOfFirstFeature {
	case .FirstValue:
    ...
	default:
    ...
}
// OR
if Bumper.nameOfSecondFeature { ...
```

Enum types also come with a helper to create them using enum position:
```swift
let firstFeature = NameOfFirstFeature.fromPosition(0)
```



## Author

Eli Kohen, eli.kohen@letgo.com

## License

bumper is available under the MIT license. See the LICENSE file for more info.

