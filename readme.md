# Title (replace with your title)
This coding tutorial will go over how a regex functions (regular expression) and what sort of use-cases it is useful for. In this example, we will be using our regex to search for phone numbers (which have an xxx-xxx-xxxx format) without specifying every single possible combination as that would be extremely slow and tedious.

## Summary

A regex is essentially a search query looking for a specific pattern. it can be used to find specific keywords in a block of text, or a find and replace function, or (and most commonly) to validate some sort of input. the regular expression used in this example is - /^\s*(?:\+?(\d{1,3}))?[-. (]*(\d{3})[-. )]*(\d{3})[-. ]*(\d{4})(?: *x(\d+))?\s*$/ - which is a regular expression that will help us search for phone numbers by looking for the specific phone number format, which is xxx-xxx-xxxx

## Table of Contents

- [Anchors](#anchors)
- [Bracket Expressions](#bracket-expressions)
- [Grouping Constructs](#grouping-constructs)
- [Quantifiers](#quantifiers)
- [Character Escapes](#character-escapes)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Breakdown](#breakdown)

## Regex Components

### Anchors
in a regex, an anchor is a character that begins and ends the parameters of the expression. The beginning and end of a regex is indicated by a forward slash (/), while the carat (^) and dollar sign ($) symbols indicate the beginning and end of a string, respectively. Anchors help assert that the engine's current position in the string matches a well-determined location, such as the beginning of a string.

in our example: /^\s*(?:\+?(\d{1,3}))?[-. (]*(\d{3})[-. )]*(\d{3})[-. ]*(\d{4})(?: *x(\d+))?\s*$/
- the / at the beginning and end indicate the bounds of the regular expression
- the ^ tells the engine to begin looking at the beginning of a line, and signifies a match will include the characters that follow it
- the $ tells the engine to stop looking at the end of the line, and signifies a match will include the characters that precede it

### Bracket Expressions
In a regex, a bracket expression is anything that is inside of a set of square brackets '[]'. This is a way to include a set of specific characters in a specific position. as an example, with the brackets [a-z0-9] the engine will match any string that includes a combination of lowercase letters from a-z and digits from 0-9

In our example: /^\s*(?:\+?(\d{1,3}))?[-. (]*(\d{3})[-. )]*(\d{3})[-. ]*(\d{4})(?: *x(\d+))?\s*$/ we have a repeating bracket expression that looks something like this: [-. ] - this regex will look for any hyphens or periods, and its positioned in our expression between the selections for the digits. this means the bracket expression is intended to match any numbers that look like 333-333-3333 or 333.333.3333

### Grouping Constructs
In a regex, a grouping construct, or "group" is anything that is inside of a set of round brackets '()'. Groups are useful for when you want to check multiple parts of a string that must fulfill different requirements. These, like bracket expressions, are a way to include a set of specific characters in a specific position. However, unlike bracket expressions, grouping constructs are very strict about what they can match, and will always look for an exact match unless told to do otherwise

In our example: /^\s*(?:\+?(\d{1,3}))?[-. (]*(\d{3})[-. )]*(\d{3})[-. ]*(\d{4})(?: *x(\d+))?\s*$/ we have 3 groups which look something like this: (\d{3}). each of these groups correlates to a section of digits in a phone number, with the \d specifying that it's looking for any digits from 0-9 and the {3} specifying that it's looking for 3 digits. since each group correlates to a section of digits in a phone number, in the example 111-222-3333, each group would look like this:
- group 1: 111
- group 2: 222
- group 3: 3333

### Quantifiers
In a regex, a quantifier sets the limits of the string that your regex, or section of regex, matches. quantifiers are inhernetly greedy, which means that they will match as many occurances of the specified pattern as possible. examples of regex include: * - matches the pattern zero or more times; + - matches the pattern one or more times; ? - matches the pattern zero or one time; {n} - matches the pattern exactly n number of times; {n,} - matches the pattern at least n number of times; {n,x} - matches the pattern from a minimum of n number of times to a maximum of x number of times

In our example: /^\s*(?:\+?(\d{1,3}))?[-. (]*(\d{3})[-. )]*(\d{3})[-. ]*(\d{4})(?: *x(\d+))?\s*$/ 
- we have a few groups containing a ? quantifier at the end, example: (?:\+?(\d{1,3}))? - this quantifier makes the preceding group optional and only counted once, as it will try to match it between zero and one times. this accounts for numbers with a country code, such as +12223334444
- we have a few groups containing a * quantifier at the end, example: [-. )]* - this quantifier makes the preceding bracket expression optional as well as counting all characters inside of the square brackets as many times as they appear, as it will try to match between zero and infinite times. this accounts for numbers with multiple special characters in-between each group, such as (222)-333-4444

### Character Escapes
In a regex, we have certain characters that signify a specific operation. For example, the + icon is a quantifier which signifies a pattern to be matched one or more times. However, what if we want the engine to match an actual + icon? That is where escapes come in. Placing a backslash (\) before an operator makes the engine look for the character that would normally correlate to an operator

In our example, /^\s*(?:\+?(\d{1,3}))?[-. (]*(\d{3})[-. )]*(\d{3})[-. ]*(\d{4})(?: *x(\d+))?\s*$/ we have a \+ escape which escapes the plus icon so that the engine looks for a literal plus icon. This is because it correlates to a country code, and country codes begin with a +,for example with +12223334444

### Character Classes
In a regex, there are certain groups characters that we may be using often in various expressions or even multiple times in the same expression. Character classes are shorthand operators for some of these character groups that are universally used very often. A few of these are: \d - which matches any digit from 0-9; \w - which matches any latin alphabet character; \s - which matches a single linespace character

In our example, /^\s*(?:\+?(\d{1,3}))?[-. (]*(\d{3})[-. )]*(\d{3})[-. ]*(\d{4})(?: *x(\d+))?\s*$/ we have a character class that is often repeated, \d. This character class is used whenever we are looking for the digits in a phone number, which can be any digit from 0-9, as the class specifies

### The OR Operator
In a regex, you might want to be able to match between one set of characters or another. While this can be done with a bracket expression, another way to achieve this could be through an OR operator. (a|b|c) is the same as [abc], and both will look for any string that includes those characters in any order for an infinite character limit. abc, acb, cabb, cc, aabbcc, all would match

In our example, we unfortunately do not have any or operators

### Flags
In a regex, you might recall that the beginning and end of a regex expression is marked by a forward slash (/) however, there is one component that can be placed outside of a regex, and that is a flag. A flag is placed at the end of the expression, and they define additional functionality/limits for the regex that will apply to the entire expression. A few examples of these are: g - global search; i - case-insensitive search; and m - multi-line search

In our example, we unfortunately do not use any flags. 

### Breakdown
Here is an in-depth breakdown of our example regular expression (not by me, from a kind fellow on stack overflow: https://stackoverflow.com/questions/16699007/regular-expression-to-match-standard-10-digit-phone-number)

/^\s*(?:\+?(\d{1,3}))?[-. (]*(\d{3})[-. )]*(\d{3})[-. ]*(\d{4})(?: *x(\d+))?\s*$/

^\s*                #Line start, match any whitespaces at the beginning if any.

(?:\+?(\d{1,3}))?   #GROUP 1: The country code. Optional.

[-. (]*             #Allow certain non numeric characters that may appear between the Country Code and the Area Code.

(\d{3})             #GROUP 2: The Area Code. Required.

[-. )]*             #Allow certain non numeric characters that may appear between the Area Code and the Exchange number.

(\d{3})             #GROUP 3: The Exchange number. Required.

[-. ]*              #Allow certain non numeric characters that may appear between the Exchange number and the Subscriber number.

(\d{4})             #Group 4: The Subscriber Number. Required.

(?: *x(\d+))?       #Group 5: The Extension number. Optional.

\s*$                #Match any ending whitespaces if any and the end of string.


## Author
My name is Mat, and I like to code

github: https://github.com/IMadeThisJustToPostThis