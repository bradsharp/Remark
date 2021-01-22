# Remark
A simple backwards-compatible alternative to markdown.

## Let's talk about Markdown

Markdown was released 16 years ago as an easy-to-read, easy-to-write format that could be converted to HTML. It's become increasingly popular over the years among bloggers, programmers and forumers. Even many of today's instant messaging apps support some flavor of it:
- Messenger
- WhatsApp*
- Telegram

Unfortunately, it's not all so great as I found out while trying to write a generic parser in Lua. There's all these different special cases you need to account for, and some features are arguably more confusing than useful. 

Take text modifiers for example. They follow some seemingly arbirtary set of rules which lead to ambiguitity and inconsistency:

- Bold and italic both use `*` or `_` where the number of symbols denotes the modifier.
- Strikethrough uses exactly two `~`.
- Monospace uses an any number of `` ` ``.
- Underline is not supported.

Now some of you might be sitting there thinking "Why didn't you just use an existing parser?". Had one existed that was written in pure Lua and didn't output HTML then I would have.

Infact, of the parsers that do exist, most don't even agree on how to format certain markdown blocks. If you've ever used a split-view markdown editor then you probably know what I'm talking about. In some cases you'll find the highlighted syntax does not match the markdown preview. [Toast UI](https://ui.toast.com/) actually did a great job explaining why this is already so you should definitely check out [their article](https://medium.com/@toastui/the-need-for-a-new-markdown-parser-and-why-e6a7f1826137) if you want to learn more.

## Why Remark?
Given the lack of standardization, annoying special-cases and missing features it was clear a better alternative to Markdown was needed. Enter Remark.

Remark has a few simple goals:
- A format everyone agrees on.
- No ambiguity in the syntax.
- Backwards compatible with existing markdown parsers.
- Extensible with other features.
- Easy to use for non-HTML usecases.

When I say backwards compatible, I mean mostly backwards compatible. It would be difficult to achieve the other goals without some changes to the syntax. However, it should be possible to create a document in Remark that looks almost identical when formatted with a Markdown parser.

## Differences

### Modifiers
In Remark each modifier has a unique symbol, this avoids any ambiguity when rendering them.

Modifier | Remark | Markdown
--- | --- | ---
Bold | `!!Bold!!` | `**Bold**` or `__Bold__`
Italic | `**Italic**` | `*Italic*` or `_Italic_`
Underline | `__Underline__` | Unsupported
Strikethrough | `~~Strikethrough~~` | `~~Strikethrough~~`
Monospace | ``` ``Monospace`` ``` | `` `Monospace` ``

Most parsers will require you to use two more symbols to modify text however this can be changed in the specific parser's settings if necessary.

### Identation

With Remark, you are able to control the indentation of your document. Starting a line with either a tab or four spaces will indent the contents by one level. Repeating this will result in the block being indented further. In contrast, Markdown would format this as a block of code. In Remark you should use backticks to do this.

### References

References are always formatted as `Symbol[Format](Source)` in Remark which makes them almost identical to Markdown. The key difference is that you can define any symbol to act as a custom reference type allowing you to extend Remark to fit your needs.

By default, Remark maps each symbol onto the following reference type:

Symbol | Type
--- | ---
` ` | Link
`!` | Media
`$` | Currency
`:` | Date

## Footnote
Yes, I realize the irony that this is written in Markdown. When GitHub supports Remark you can be sure I'll update it.
