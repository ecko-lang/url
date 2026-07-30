# URL - Ecko Std Lib Package

URL parsing, query strings, and reference resolution for [Ecko](https://ecko.sh),
written in Ecko.

## Install

```bash
ecko get github.com/ecko-lang/url
```

## Usage

```ecko
import url

p = url.parse("https://ada@ecko.sh:8080/docs?section=intro#top")
# { scheme: "https", userinfo: "ada", host: "ecko.sh", port: 8080,
#   path: "/docs", query: "section=intro", fragment: "top" }

url.query_parse("a=1&b=two+words")   # { a: "1", b: "two words" }
url.query_build({ a: "1", b: "x y" }) # "a=1&b=x%20y"  (keys sorted)

url.join("https://ecko.sh/a/b/c", "../x")  # "https://ecko.sh/a/x"  (RFC 3986)
url.build(p)                                # recompose -> the URL string

url.encode("a b/c")   # "a%20b%2Fc"
url.decode("a%20b")   # "a b"
```

## API

| Function | Description |
|---|---|
| `parse(url)` | `{ scheme, userinfo, host, port, path, query, fragment }` (`port` is an Int or `null`) |
| `build(parts)` | Recompose a URL string from a components map (inverse of `parse`) |
| `query_parse(str)` | Parse a query string to a map; `+` and `%xx` are decoded |
| `query_build(map)` | Build a query string; keys sorted, values percent-encoded |
| `join(base, ref)` | Resolve `ref` against `base` (RFC 3986 §5, with `.`/`..` handling) |
| `encode(s)` / `decode(s)` | Percent-encode / decode (re-exported from `std.encoding`) |

## Notes

- `parse` uses the RFC 3986 Appendix B regex, so it never fails - an
  unusual string just yields mostly-empty components.
- `join` implements the RFC reference-transform, including dot-segment removal.
- IPv6 host literals (`[::1]`) are not specially handled in v1.

## Testing

```bash
ecko test tests/
```

Offline and deterministic; `example.ecko` is a runnable demo.

## License

MIT - see [LICENSE](LICENSE).
