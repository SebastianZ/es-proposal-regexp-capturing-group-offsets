# Advanced information for regular expression matches

ECMAScript proposal, specs and tests for advanced regular expression matches information.

## Rationale

ECMAScript allows to match a regular expression against a string, which returns the matched chunks. For further processing you sometimes additionally require the offsets of those matches, which are not exposed by the current APIs.

## Proposed solution

### Add `RegExp.prototype.execAdvanced()`

```javascript
RegExp.prototype.execAdvanced(str)
```

#### Values
- `str`

 The string against which to match the regular expression.

#### Return value
An array containing objects with information about the matches. The match objects contain different properties:

- `match`

 A string containing the matched result.

- `start`

 An integer representing the index of the first character of the matched result within the input string.

- `end`

 An integer representing the index after the last character of the matched result within the input string.

- `groups`

 An array of objects containing the match information for captured groups. The objects contain the properties `match`, `start` and `end` like the main match object.

#### Examples

##### Non-global regular expression

```javascript
let matches = /a(b(c))/.execAdvanced("abcabc");
```

Then `matches` will contain this:

```javascript
[
  {
    match: "abc",
    start: 0,
    end: 3,
    groups: [
      {
        match: "bc",
        start: 1,
        end: 3
      },
      {
        match: "c",
        start: 2,
        end: 3
      }
    ]
  }
]
```

##### Global regular expression

```javascript
let matches = /a(b(c))/g.execAdvanced("abcabc");
```

Then `matches` will contain this:

```javascript
[
  {
    match: "abc",
    start: 0,
    end: 3,
    groups: [
      {
        match: "bc",
        start: 1,
        end: 3
      },
      {
        match: "c",
        start: 2,
        end: 3
      }
    ]
  },
  {
    match: "abc",
    start: 3,
    end: 6,
    groups: [
      {
        match: "bc",
        start: 4,
        end: 6
      },
      {
        match: "c",
        start: 5,
        end: 6
      }
    ]
  }
]
```

### Add `String.prototype.matchAdvanced()`

#### Syntax
```
String.prototype.matchAdvanced(regexp)
```

#### Values
- str
  The string against which to match the regular expression.

#### Return value
An array containing objects with information about the matches. The match objects contain different properties:

- match
 A string containing the matched result.
- start
 An integer representing the index of the first character of the matched result within the input string.
- end
 An integer representing the index after the last character of the matched result within the input string.
- groups
 An array of objects containing the match information for captured groups. The objects contain the properties `match`, `start` and `end` like the main match object.

#### Examples

##### Non-global regular expression

```javascript
let matches = "abcabc".matchAdvanced(/a(b(c))/);
```

Then `matches` will contain this:

```javascript
[
  {
    match: "abc",
  start: 0,
  end: 3,
  groups: [
    {
      match: "bc",
    start: 1,
    end: 3
    }
    {
      match: "c",
    start: 2,
    end: 3
    }
  ]
  }
]
```

##### Global regular expression

```javascript
let matches = "abcabc".matchAdvanced(/a(b(c))/g);
```

Then `matches` will contain this:

```javascript
[
  {
    match: "abc",
    start: 0,
    end: 3,
    groups: [
      {
        match: "bc",
        start: 1,
        end: 3
      },
      {
        match: "c",
        start: 2,
        end: 3
      }
    ]
  },
  {
    match: "abc",
    start: 3,
    end: 6,
    groups: [
      {
        match: "bc",
        start: 4,
        end: 6
      },
      {
        match: "c",
        start: 5,
        end: 6
      }
    ]
  }
]
```
