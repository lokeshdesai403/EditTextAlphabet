# EditTextAlphabet
If you don't want to allow users to enter digits and special character into edittext using mobile keyboard then you can use this code.

## How to make EditText accepts only Alphabet(No digits or Special Characters)?

I made **AlphabetsSymbolsInputFilter** class which you can use with your EditText. Even you can specify special characters which you want to allow from user.

```
class AlphabetsSymbolsInputFilter(symbols: String) : InputFilter {

        private var mWordPattern: String
        var mLetterPattern: String

        init {
            mLetterPattern = "[a-zA-Z.$symbols ]"
            //mLetterPattern = "[a-zA-Z0-9.$symbols ]" // replace if alphanumeric
            mWordPattern = "$mLetterPattern+"
        }

        override fun filter(
            source: CharSequence,
            start: Int,
            end: Int,
            dest: Spanned,
            dstart: Int,
            dend: Int
        ): CharSequence? {
            if (source == "") {
                println("In backspace")
                return source
            }
            if (source.isNotEmpty() && source.toString().matches(mWordPattern.toRegex())) {
                return source
            }
            var sourceStr = ""
            if (source.isNotEmpty() && !source.toString().matches(mLetterPattern.toRegex())) {
                sourceStr = source.toString()
                while (sourceStr.isNotEmpty() && !sourceStr.matches(mWordPattern.toRegex())) {
                    println(" source --> $source dest ---> $dest")
                    if (sourceStr.last().isDigit()) {
                        print("Is digit ")
                        sourceStr = sourceStr.subSequence(0, sourceStr.length - 1).toString()
                    } else if (!sourceStr.last().toString().matches(mLetterPattern.toRegex())) {
                        print("Is emoji or weird symbols")
                        sourceStr = sourceStr.subSequence(0, sourceStr.length - 1).toString()
                    } else
                        break
                }
                return sourceStr
            }
            return source
        }
    }

```

Use below function with EditText filter.

```
  fun editTextAllowAlphabetsSymbols(symbols: String): Array<InputFilter> {
        return arrayOf(AlphabetsSymbolsInputFilter(symbols))
    }
```

## How to use AlphabetsSymbolsInputFilter with EditText?

Just need to call **editTextAllowAlphabetsSymbols()** function with EditText's Filter Property.

```
  editTextName.filters = editTextAllowAlphabetsSymbols("")
```
If you want to allow user to use special character like '$' than you can do like below.

```
 editTextName.filters = editTextAllowAlphabetsSymbols("$")
```

## Where to use AlphabetsSymbolsInputFilter class?

You can use when taking user input for 

* Name, First Name and Last Name
* Birthplace
* State
or any places when you don't want to allow user to add digit and special characters.

## Blogs

You can check my other blogs on [Android4dev](https://android4dev.com). If you like my work than hit **STAR** located on Top Right corner.

## Author Contact

[Lokesh Desai](https://github.com/lokeshdesai403)

[Twitter](https://twitter.com/lokesh_desai)
