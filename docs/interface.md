# The Interface

All interactions with the Exercism website are handled automatically. Representers have the single responsibility of taking a solution and returning a representation of it. See the [introduction](./introduction.md) for more information.

## Execution

- A Representer should provide an executable script. You can find more information in the [docker.md](../docker.md) file.
- The script will receive two parameters:
  - The slug of the exercise (e.g. `two-fer`).
  - A path to a directory containing the submitted file(s) (with a trailing slash).
- The script must write a `representation.txt` file to that directory.
- The script must write a `mapping.json` file to that directory.

## Output format

The `representation.txt` file contains some sort of canonical representation. This representation can take many forms, but is usually an AST:

```ruby
s(:class,
  s(:const, nil, :PLACEHOLDER_1), nil,
  s(:begin,
    s(:def, :PLACEHOLDER_2,
      s(:args),
      s(:begin,
        s(:lvasgn, :PLACEHOLDER_3,
          s(:str, "foo")),
        s(:lvasgn, :PLACEHOLDER_4,
          s(:str, "foo")),
        s(:return,
          s(:lvar, :PLACEHOLDER_3)))),
    s(:def, :PLACEHOLDER_3,
      s(:args),
      s(:const, nil, :PLACEHOLDER_1))))
```

The representation could also just be (normalized) source code:

```ruby
class PLACEHOLDER_1
  def PLACEHOLDER_2
    PLACEHOLDER_3 = "foo"
    PLACEHOLDER_4 = "foo"
    return PLACEHOLDER_3
  end

  def PLACEHOLDER_3
    PLACEHOLDER_1
  end
end
```

The `mapping.json` file maps placeholders to their original values:

```json
{
  "PLACEHOLDER_1": "TwoFer",
  "PLACEHOLDER_2": "two_fer",
  "PLACEHOLDER_3": "foo",
  "PLACEHOLDER_4": "bar"
}
```

It is important to note that all identical names must be replaced with the same placeholder, irrespective of scope.

## Debugging

The contents of `stdout` and `stderr` from each run will be persisted into files that can be viewed later.

You may write an `representation.out` file that contains debugging information you want to later view.

At a later date, we will provide an interface for you to download these files along with the submitted files, `representation.txt` and `mapping.json`.
