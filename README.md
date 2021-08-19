Install OSQP (Octave)
=====================

### GitHub Action to build and install the [OSQP][1] interface for
[GNU Octave][2]

Builds and installs the [Octave][2] interface for [OSQP][1] and defines
the `OSQP_PATH` environment variable to point to the directory
containing the [OSQP][1] MEX- and M-files.

__Note:__ This action depends on [Octave][2] being installed first, using either
[`MATPOWER/action-install-octave-linux@v1`][3] or
[`MATPOWER/action-install-octave-macos@v1`][4].

Tested on Linux and macOS runners.

### Optional Input

- `cached` - (default `false`) set to `true` to indicate that you are
  installing from a cached build, i.e. skip the building and just define
  `OSQP_PATH` and install it in the Octave path.

### Outputs

None.

### Example Usage

```
    steps:
    - name: Install Octave
      uses: MATPOWER/action-install-octave-linux@v1

    - name: Octave ${{ env.ML_VER }} Installed
      run: $ML_CMD ver

    - name: Cache OSQP interface for Octave
      id: cache-osqp
      env:
        cache-name: osqp
      uses: actions/cache@v2
      with:
        path: ~/build/osqp-matlab
        key: ${{ matrix.os }}-${{ env.cache-name }}

    - name: Install OSQP interface for Octave
      uses: MATPOWER/action-install-osqp-octave@v1
      with:
        cached: ${{ steps.cache-osqp.outputs.cache-hit == 'true' }}

    - name: Run OSQP code in Octave
      run: |
        export OSQP_TEST_PATH=<directory-with-code-that-calls-OSQP>
        env $ML_PATHVAR=$OSQP_TEST_PATH $ML_CMD "<code-that-calls-OSQP>"
        ls -al $OSQP_PATH
```

[1]: https://osqp.org
[2]: https://octave.org
[3]: https://github.com/MATPOWER/action-install-octave-linux
[4]: https://github.com/MATPOWER/action-install-octave-macos
