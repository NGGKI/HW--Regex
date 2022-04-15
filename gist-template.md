# Title (Overview of Regex)

A regular expression known as regex or regexp is a sequence of characters that specifies a search pattern in text. Usually such patterns are used by string-searching algorithms for "find" or "find and replace" operations on strings, or for input validation. It is a technique developed in theoretical computer science and formal language theory.

## Summary

Briefly summarize the regex you will be describing and what you will explain. Include a code snippet of the regex. Replace this text with your summary.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components

### Anchors

The ^ anchor signifies a string that begins with the characters that follow it. This could be in one of two formats:

An exact string match, such as ^The, where the strings "The" or "The person" match, but "the" and "the person" do not. This is because a regex is case-sensitive.

A range of possible matches, displayed using bracket expressions. We'll discuss this in the next section.

The $ anchor signifies a string that ends with the characters that precede it. Just as with the ^ character, it can be preceded by an exact string or a range of possible matches.

So in our “Matching a Username” regex, the string must start and end with something that matches the pattern [a-z0-9_-]. You'll notice that we didn't include the pattern {3,16}, which precedes the $ character. Why? Because this is a special component called a quantifier. We'll come back to quantifiers in a moment.

Let's take a look at the pattern [a-z0-9_-] and see what it means.

### Quantifiers

All right, so now we know what we're looking for inside the brackets. What about the last requirement of the regex ("is between 3–16 characters long")? And what about that strange sequence of characters that appeared before the $ anchor ({3,16}). To answer these questions, let's take a look at quantifiers.

Quantifiers set the limits of the string that your regex matches (or an individual section of the string). They frequently include the minimum and maximum number of characters that your regex is looking for.

Quantifiers are inherently greedy, meaning they match as many occurrences of particular patterns as possible. They include the following:

\*—Matches the pattern zero or more times

+—Matches the pattern one or more times

?—Matches the pattern zero or one time

{}—Curly brackets can provide three different ways to set limits for a match:

{ n }—Matches the pattern exactly n number of times

{ n, }—Matches the pattern at least n number of times

{ n, x }—Matches the pattern from a minimum of n number of times to a maximum of x number of times

Each of these quantifiers can be made lazy by adding the ? symbol after it, meaning it will match as few occurrences as possible.

Now let’s look at how quantifiers are used in the “Matching a Username” regex. There, we have the quantifier {3,16}. This means that we want to find the preceding string pattern a minimum of 3 times and a maximum of 16 times. Because our bracket expression ([a-z0-9_-]) will match any string that includes any combination of lowercase letters between a and z, any number between 0 and 9, and the special characters of an underscore or a hyphen, this quantifier means that this string has to be between 3 and 16 characters.

Let's return to our entire regex:

/^[a-z0-9_-]{3,16}$/
This regex is looking for any string between 3 and 16 characters that starts and ends with a combination of lowercase characters, the numbers 0–9, and the special characters of an underscore and a hyphen.

The following examples match:

123

l3rn-antin0_1

The 123 string is a match because it meets the minimum requirement of 3 characters, and the characters are all numbers. l3rn-antin0_1 is a match because it is 13 characters long (within the minimum and maximum character limits of 3 and 16) and includes a combination of lowercase characters, numbers, and the special characters of an underscore and a hyphen. Remember that because our regex pattern is entirely inside a bracket expression, the string doesn’t need to meet all of the requirements—just any of them.

The following examples do not match:

25

L3RN-antino0_1

l3rn-antin0_am1k0

l3rn\*antin0?1

The 25 string is not a match because it only contains 2 characters and the minimum is 3. L3RN-antino0_1 includes uppercase characters. l3rn-antin0_am1k0 has 17 characters, and the maximum limit for this regex is 16. l3rn*antin0?1 includes the special characters * and ?, which aren't allowed in this regex.

Great! We've successfully explained the “Matching a Username” regex. But is that all there is to learn about regular expressions? This actually only scratches the surface of what we can accomplish by using a regex. Although it's beyond the scope of this tutorial to learn everything, let’s touch on a few other regex components that you might encounter.

### Grouping Constructs

The “Matching a Username” regex is fairly straightforward and open-ended about what it accepts. As regular expressions grow more complicated, you may check multiple parts of a string to determine that different sections fulfill different requirements. To break these sections up, you'll need to use grouping constructs.

The primary way you group a section of a regex is by using parentheses (()). Each section within parentheses is known as a subexpression.

The following example contains two grouping constructs or subexpressions:

(abc):(xyz)
The first subexpression is looking for a part of the string that matches the string "abc" exactly. Similarly, the second subexpression is looking for "xyz". In between the subexpressions, we have a colon (:). Thus, the string "abc:xyz" would match, but the string "acb:xyz" would not. Unlike bracket expressions, subexpressions look for an exact match unless they're told to do otherwise.

Grouping constructs have two primary categories: capturing and non-capturing. The details about how capturing and non-capturing groups are used are beyond the scope of this tutorial. The important thing to understand is that capturing groups capture the matched character sequences for possible re-use (including a numbered backreference) while non-capturing groups do not. A grouping construct can be made non-capturing by adding the characters ?: at the beginning of an expression inside the parentheses.

### Bracket Expressions

Bracket Expressions
Anything inside a set of square brackets ([]) represents a range of characters that we want to match. These patterns are known as bracket expressions, but they are also known as a positive character group, because they outline the characters we want to include. We can write these expressions to include all of the characters we want to match. For example, [abc] will look for a string that includes a or b or c, regardless of the length of the string. So all of the following examples would match: "aaa", "bin" "court", "abracadabra", and "bca".

You'll more commonly see a hyphen (-) used between alphanumeric characters (letters and numbers only) to represent a range of those possible characters. This means that [a-c] and [abc] will look for the exact same thing.

In our “Matching a Username” regex example, we can break down the bracket expressions as follows:

[a-z]—The string can contain any lowercase letter between a–z. Keep in mind that this looks for lowercase characters only. If we wanted to include uppercase characters, we would need to change the expression to [a-zA-Z].

[0-9]—The string can contain any number between 0–9

[_-]—The string can contain an underscore or hyphen. Both the underscore and the hyphen are called special characters. Special characters include any non-alphanumeric characters, such as punctuation or symbols. In this case, we only want a string that includes \_ or -. It's important to note that the hyphen here is not the same hyphen that we used in our alphanumeric ranges. In bracket expressions, special characters that we want to include follow alphanumeric character ranges within the brackets.

If we put all of these expressions together so that our pattern is [a-z0-9_-], this will match any string that includes any combination of lowercase letters between a and z, any number between 0 and 9, and the special characters of an underscore or a hyphen. Keep in mind that these characters can be in any order. It's also important to note that this pattern does not require the string to meet all of these requirements; it can meet any of them.

The following examples fulfill the requirements of this regex:

"lernantino"

"lernantino1"

"l3rnantino_1"

"lern-antino"

"21452"

"_-_-"

What about the string "Lernantino"? This would not match our pattern because it includes an uppercase character, L.

It's important to note that a bracket expression can be turned into a negative character group by adding the ^ symbol to the beginning of the expression inside the brackets. A common example is matching a string that doesn't include any vowels. The pattern [^aeiouaeiou] would find any strings that don't include lowercase or uppercase vowels.

### Character Classes

A character class in a regex defines a set of characters, any one of which can occur in an input string to fulfill a match. We've actually already discussed some character classes. The bracket expressions outlined previously, including positive and negative character groups, are considered character classes.

Here are some of the other common character classes:

.—Matches any character except the newline character (\n)

\d—Matches any Arabic numeral digit. This class is equivalent to the bracket expression [0-9].

\w—Matches any alphanumeric character from the basic Latin alphabet, including the underscore (_). This class is equivalent to the bracket expression [A-Za-z0-9_].

\s—Matches a single whitespace character, including tabs and line breaks

Each of the last three character classes can be changed to perform an inverse match by capitalizing the letter character. For example, \D matches a non-digit character.

### The OR Operator

Remember that a bracket expression does not require the string to meet all of the requirements in the pattern. This means that [a-z0-9_-] searches for alphanumeric characters or the two special characters included in the pattern. Often, you'll want to add this same logic outside of a bracket expression, especially within a grouping construct or between two different grouping constructs.

Using the OR operator (|), the expression [abc] could be written as (a|b|c). Using our example in the grouping constructs section, we can take the original expression:

(abc):(xyz)
And then use the OR operator to convert it to the following:

(a|b|c):(x|y|z)
Now, both of the strings "abc:xyz" and "acb:xyz" would match, as well as "a:z", but "xyz:abc" would not.

### Flags

We started this tutorial by explaining that as a literal, a regex must be wrapped in slash characters. The one exception to this rule is with the component known as flags. Flags are placed at the end of a regex, after the second slash, and they define additional functionality or limits for the regex. There are six optional flags that can be used, either separately or together and in any order, but these are the three you're most likely to encounter:

g—Global search: the regex should be tested against all possible matches in a string.

i—Case-insensitive search: case should be ignored while attempting a match in a string

m—Multi-line search: a multi-line input string should be treated as multiple lines

### Character Escapes

The backslash (\) in a regex escapes a character that otherwise would be interpreted literally. For example, the open curly brace ({) is used to begin a quantifier, but adding a backslash before the open curly brace (\{) means that the regex should look for the open curly brace character instead of beginning to define a quantifier. This is common when looking for strings with special characters that are the same as a particular component of a regex.

It's important to note that all special characters, including the backslash (\), lose their special significance inside bracket expressions.

## Author

You can find more about regex on https://www.regular-expressions.info/

Github: https://github.com/NGGKI/HW-Regex
