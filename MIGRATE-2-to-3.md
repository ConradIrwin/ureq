# Change ureq 2.x -> 3.x

## HTTP Crate

In 2.x ureq implemented it's own `Request` and `Response` structs. In 3.x, we
drop our own impl in favor of the [http crate]. The http crate presents a unified HTTP
API and can be found as a dependency of a number of big [http-related crates] in the
Rust ecosystem. The idea is that presenting a well-known API towards users of ureq
will make it easier to use.

* .set -> .header

```
// ureq2.x
ureq::get("https://httpbin.org/get").set("foo", "bar").call();
// ureq3.x
ureq::get("https://httpbin.org/get").header("foo", "bar").call();
```

## Re-exported crates must be semver 1.x (stable)

ureq2.x re-exported a number of semver 0.x crates and thus suffered from that breaking
changes in those crates technically were breaking changes in ureq (and thus ought to increase
major version). In ureq 3.x we will strive to re-export as few crates as possible.

* No re-exported tls config
* No re-exported cookie crates
* No re-exported json macro

Instead we made our own TLS config and Cookie API, and drop the json macro.



[http crate]         : https://crates.io/crates/http
[http-related crates]: https://crates.io/crates/http/reverse_dependencies

* 2.x charset feature controlled lossy vs encoding_rs.
* into_string() -> read_to_string()
* native-certs is gone. native roots are always available.
* lossy utf-8 always enabled also when not charset feature
* agent builder
* no retry idempotent (for now)
* no send body charset encoding (for now)
