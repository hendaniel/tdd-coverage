
## Demo of unit testing report tracking

Example:
Go to this [PR](https://github.com/hendaniel/tdd-coverage/pull/1)

Workflow:

    name: Reporting unit test coverage
    on: [push]
    jobs:
      run:
        runs-on: ubuntu-latest
        strategy:
          matrix:
            node-version: [15.x]    
        steps:
        - uses: actions/checkout@master
        - name: Use Node.js ${{ matrix.node-version }}
          uses: actions/setup-node@v2
          with:
            node-version: ${{ matrix.node-version }}
        - name: Generate coverage report
          run: |
            npm ci
            npm test -- --coverage --watchAll=false
        - name: Upload coverage to Codecov
          uses: codecov/codecov-action@v2
          with:
            fail_ci_if_error: true
            files: ./coverage/clover.xml
            flags: unittests
            name: codecov-umbrella
