# pip-compile-inconsistent-via-backtracking-resolver

Demonstrate an issue with [pip-tools](https://github.com/jazzband/pip-tools) where using backtracking resolver and
constrained requirements leads to inconsistent output for `via` comments.

- [`9142bf4`](https://github.com/mkniewallner/pip-compile-inconsistent-via-backtracking-resolver/commit/9142bf4bb0fa985ddd09565b1195b889a3ee3a4b)
  adds the 2 requirements input files
- [`8035ad9`](https://github.com/mkniewallner/pip-compile-inconsistent-via-backtracking-resolver/commit/8035ad90a9e32aae6529b7ff30d86536ab1839dd)
  generates `requirements.txt` from `requirements.in` by running `pip-compile --resolver=backtracking requirements.in`
- [`9d2c503`](https://github.com/mkniewallner/pip-compile-inconsistent-via-backtracking-resolver/commit/9d2c50398f9499719509349a295424f6699bac2d)
  generates the not yet existing `requirements-dev.txt` from `requirements-dev.in` by
  running `pip-compile --resolver=backtracking requirements-dev.in`
- [`798d51a`](https://github.com/mkniewallner/pip-compile-inconsistent-via-backtracking-resolver/commit/798d51a6c06da0863ae86941d3a460865ea17e78)
  updates the existing `requirements-dev.txt` from `requirements-dev.in` by
  running `pip-compile --resolver=backtracking requirements-dev.in` again

The first time `requirements-dev.txt` is generated, `via` comments correctly indicate that `anyio`, `idna` and `sniffio`
are constrained by `requirements.txt`, but when re-running the command with an already existing `requirements-dev.txt`
file, the references to `requirements.txt` are removed.
