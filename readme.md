# Choose from variables

Github action to extract information from environment variables or secrets based on an input.

## Usage

The general usage looks like this:

```yml
name: Test choosing from variables

on:
  workflow_dispatch:
    inputs:
      environment:
        description: 'Environment'
        required: true
        type: choice
        options:
          - develop
          - production

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - name: Choose the variable
      uses: berthott/choose-from-variables@v1
      with:
        variable: ${{ vars.TEST_VARIABLES }}              # The environment variable defined in github
        output_name: TEST_VARIABLE                        # How your output should be named.
        selection: ${{ github.event.inputs.environment }} # The selection to make from inside your variable
    - name: Use the variable directly
      run: echo $TEST_VARIABLE
    - name: Or use it via the env
      run: echo ${{ env.TEST_VARIABLE }}
```

With an repository variable defined inside Github with the name of `TEST_VARIABLES`.
By default this variable expects single lines according to your options, eg.:

```env
develop="Development Test"
production="Production Test"
```

You could choose to use you variable with multiple lines, and separate them with a clear line, eg.:

```env
develop=
LINE1=Something
LINE2=Something

production=
LINE1="Something else"
LINE2="Something else"
```

To correctly extract this variable you need to define `multi_line: true` as an input, eg.:

```yml
with:
  variable: ${{ vars.TEST_VARIABLES }}
  output_name: TEST_VARIABLE
  selection: ${{ github.event.inputs.environment }}
  multi_line: true
```

If you use single instead of double quotes for values within you variables set `single_quotes: true` to preserve them, eg.:


```yml
with:
  variable: ${{ vars.TEST_VARIABLES }}
  output_name: TEST_VARIABLE
  selection: ${{ github.event.inputs.environment }}
  multi_line: true
  single_quotes: true
```

You could also choose to use a secret instead of a variable:

```yml
with:
  variable: ${{ secrets.TEST_VARIABLES }}
  output_name: TEST_VARIABLE
  selection: ${{ github.event.inputs.environment }}
```


## License

See [License File](license.md). Copyright © 2024 Jan Bladt.