# Gemini Text Processor

This is a library and a tool to manipulate [gemini text files](https://gemini.circumlunar.space/docs/specification.html).

It provides both an easy-to-use API as well as a streaming parser with minimal allocation requirements and a proper separation between temporary allocations required for parsing and allocations for returned text fragments.

The library is thoroughly tested with a lot of gemini text edge cases and all (tested) cases are handled reasonably.

## Features

- Fully spec-compliant gemini text parsing
- Non-blocking streaming parser
- Provides both a convenient [Zig](src/gemtext.zig) and [C](include/gemtext.h) API
- Rendering to several formats
  - Gemini text
  - HTML
  - Markdown
  - RTF

## Example

This is a simple example that parses a gemini file and converts it into a HTML file.

```zig
pub fn main() !void {
    var document = try gemtext.Document.parse(
      std.heap.page_allocator,
      std.io.getStdIn().reader(),
    );
    defer document.deinit();

    try gemtext.renderer.html(
      document.fragments.items, 
      std.io.getStdOut().writer(),
    );
}
```

More examples can be found the the examples folder:
- `gem2html` ([C](examples/gem2html.c), [Zig](examples/gem2html.zig))
- `gem2md` ([C](examples/gem2md.c), [Zig](examples/gem2md.zig))
- `streaming-parser` ([C](examples/streaming-parser.c), [Zig](examples/streaming-parser.zig))