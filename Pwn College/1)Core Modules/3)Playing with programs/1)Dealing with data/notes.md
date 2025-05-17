two hex digits represent 8bits(as one hex represents 4 bits).

![[Pasted image 20250301121217.png]]
`\x` specifies 1 byte of hex chars.

Once computing went international and emojis were added, people needed to be able to use more than 256 possible characters at a time. In the modern era, this has been largely solved by the [UTF-8](https://en.wikipedia.org/wiki/UTF-8) encoding. UTF-8 is a specific multi-byte encoding of [Unicode](https://en.wikipedia.org/wiki/Unicode), a global standardized character set containing essentially all characters known to humanity, plus the fun emoji that you know and love. There are many ways to encode Unicode, and UTF-8 is one of them. Unicode (character set) is to UTF-8 (encoding) as English (character set) is to standard ASCII (encoding).Conveniently, UTF-8 is backwards-compatible with standard ASCII (e.g., standard ASCII byte values represent the same character in UTF-8 as in ASCII), but in certain situations will use _more than one byte_ to represent a single character. This allows UTF-8 to have essentially limitless character options (it can always interpret more bytes!): currently, it supports well over 1,000,000 characters!

todo: look more into base64
