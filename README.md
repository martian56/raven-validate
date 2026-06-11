# raven-validate

Field validation that collects every problem instead of stopping at the first.
You make a `Validator`, run your checks against it, then ask whether it passed.
No dependencies.

## Install

```toml
[dependencies]
"github.com/martian56/raven-validate" = "0.1"
```

## Usage

```raven
import "github.com/martian56/raven-validate" { Validator }

fun check_signup(name: String, email: String, age: Int) -> Bool {
    let v = Validator.new()
    v.required("name", name)
    v.min_len("name", name, 2)
    v.email("email", email)
    v.in_range("age", age, 13, 120)

    if v.is_valid() {
        return true
    }
    // v.errors() is a list of readable messages to show the user
    return false
}
```

The rule methods also return the validator, so chaining works if you'd rather
write it that way:

```raven
let v = Validator.new().required("name", name).email("email", email)
```

## Rules

| Method | Passes when |
|--------|-------------|
| `required(field, value)` | `value` isn't empty |
| `min_len(field, value, n)` | `value` is at least `n` characters |
| `max_len(field, value, n)` | `value` is at most `n` characters |
| `email(field, value)` | `value` has the shape of an email |
| `one_of(field, value, options)` | `value` is in `options` |
| `in_range(field, value, lo, hi)` | the `Int` `value` is within `[lo, hi]` |
| `check(ok, message)` | `ok` is true (for a rule of your own) |

The `field` name goes into the message, so you get things like `name is
required` or `age must be between 13 and 120`.

## Reading the result

`is_valid()` is the quick yes or no. `errors()` is the full list of messages,
and `first_error()` is the first one as an `Option<String>`.

## License

MIT
