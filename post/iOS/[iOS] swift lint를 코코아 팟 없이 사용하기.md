스위프트로 프로그래밍을 하면서 [swift 코딩스타일](https://github.com/StyleShare/swift-style-guide)에 맞춰서 코딩을 하도록 도와줄 장치를 찾아보니 swiftLint를 만나게 되었습니다.
사용 방법에 대한 설명을 [공식문서](https://github.com/realm/SwiftLint)에 제일 잘 나와있기 때문에 첨부만 해 놓겠습니다.

기존의 프로젝트와는 다르게 코딩의 규칙은 설치되어 있어야 하기 때문에
```
$ brew install swiftlint
```
로 설치 해줍니다.

Xcode를 실행해서 타겟에 새로운 "Run Script"을 눌러서 추가해 줍니다.
```
if which swiftlint >/dev/null; then
  swiftlint
else
  echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
fi
```

설치 후 실행하면 에러나 워닝들이 빡!  

`autocorrect` 옵션을 추가해주면, 빌드 했을 때 autocorrect 해줍니다.

```
if which swiftlint >/dev/null; then
  swiftlint autocorrect
else
  echo "warning: SwiftLint not installed, download from https://github.com/realm/SwiftLint"
fi
```

빈 파일을 추가해 줍니다. 이름은 반드시 `.swiftlint.yml`여야 합니다.
그리고 추가한 파일 안에 규칙들을 적어놓습니다.

[규칙 보러가기](https://realm.github.io/SwiftLint/rule-directory.html)

```
# By default, SwiftLint uses a set of sensible default rules you can adjust:
disabled_rules: # rule identifiers turned on by default to exclude from running
  - colon
  - comma
  - control_statement
opt_in_rules: # some rules are turned off by default, so you need to opt-in
  - empty_count # Find all the available rules by running: `swiftlint rules`

# Alternatively, specify all rules explicitly by uncommenting this option:
# whitelist_rules: # delete `disabled_rules` & `opt_in_rules` if using this
#   - empty_parameters
#   - vertical_whitespace

included: # paths to include during linting. `--path` is ignored if present.
  - Source
excluded: # paths to ignore during linting. Takes precedence over `included`.
  - Carthage
  - Pods
  - Source/ExcludedFolder
  - Source/ExcludedFile.swift
  - Source/*/ExcludedFile.swift # Exclude files with a wildcard
analyzer_rules: # Rules run by `swiftlint analyze` (experimental)
  - explicit_self

# configurable rules can be customized from this configuration file
# binary rules can set their severity level
force_cast: warning # implicitly
force_try:
  severity: warning # explicitly
# rules that have both warning and error levels, can set just the warning level
# implicitly
line_length: 110
# they can set both implicitly with an array
type_body_length:
  - 300 # warning
  - 400 # error
# or they can set both explicitly
file_length:
  warning: 500
  error: 1200
# naming rules can set warnings/errors for min_length and max_length
# additionally they can set excluded names
type_name:
  min_length: 4 # only warning
  max_length: # warning and error
    warning: 40
    error: 50
  excluded: iPhone # excluded via string
  allowed_symbols: ["_"] # these are allowed in type names
identifier_name:
  min_length: # only min_length
    error: 4 # only error
  excluded: # excluded via string array
    - id
    - URL
    - GlobalAPIKey
reporter: "xcode" # reporter type (xcode, json, csv, checkstyle, junit, html, emoji, sonarqube, markdown, github-actions-logging)
```

설정 해주고 빌드를 하면! 끝!


