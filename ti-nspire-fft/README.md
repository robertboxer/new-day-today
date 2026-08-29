# fftgraph — TI-Nspire FFT of a time-domain expression

`fftgraph.tibas` is a TI-Nspire Basic program (written for the Program
Editor). It samples a user-supplied time-domain expression such as
`sin(t)+2*cos(t)`, runs a radix-2 Cooley-Tukey FFT on the samples, and
stores the frequency-domain result as document variables so you can graph
it.

TI-Nspire Basic has no scripted "plot" command the way TI-84 Basic does
(`Plot1(...)`) — graphing on Nspire is page-based. So the program computes
everything and leaves four lists in the current Problem:

- `tlist`, `xlist` — the sampled time-domain data (t, f(t))
- `freq`, `mag` — the one-sided frequency spectrum (Hz, magnitude)

You then add a graph page in the same Problem to visualize `mag` vs `freq`.

## Installing

1. In TI-Nspire Student/Teacher Software (or on the handheld): create a
   new document, then **Insert > Program Editor**.
2. Name the program `fftgraph` and paste in the contents of
   `fftgraph.tibas`.
3. Note: the imaginary unit in the FFT twiddle factor is the special
   Nspire italic **ⅈ** symbol (from the π/𝑖 template key), not the letter
   `i`. If your editor/paste strips it, retype that character using the
   calculator's built-in `i` (imaginary unit) key/template instead of the
   keyboard letter `i`.
4. Check syntax (Menu > Check Syntax & Store) and save.

## Running

1. Run `fftgraph()` (from the Calculator page, or Ctrl+R in the Program
   Editor).
2. When prompted:
   - **f(t) =** — enter the expression in terms of `t`, e.g.
     `sin(t)+2*cos(t)`.
   - **Samples exponent k** — number of samples is `n = 2^k` (try `k=6`
     for 64 samples).
   - **Sample spacing dt** — time between samples in seconds (try `0.05`
     for a sample rate of 20 Hz).
3. The program prints "FFT complete" and creates `tlist`, `xlist`,
   `freq`, `mag` as variables in the current Problem.

## Viewing the frequency-domain graph

In the **same Problem** (Nspire variables are shared per-Problem, not
per-document):

1. Insert a **Graphs** page.
2. Open **Menu > Graph Entry/Edit > Scatter Plot** (or Function Table
   equivalent), set the X list to `freq` and the Y list to `mag`.
   - Alternatively, insert a **Data & Statistics** page: click the
     x-axis label and choose `freq`, click the y-axis label and choose
     `mag`.
3. You should see spikes at the frequencies present in your input
   expression. For `sin(t)+2*cos(t)` sampled with `dt=0.05`, expect a
   spike near `freq ≈ 1/(2π) ≈ 0.159 Hz` with magnitude close to
   `sqrt(1²+2²) ≈ 2.24` (amplitude of the combined sinusoid at that
   frequency).

## Notes / limitations

- `n = 2^k` must be a power of two (required by the radix-2 FFT); the
  program takes `k` directly so this is enforced by construction.
- The expression may reference `t` and standard Nspire functions
  (`sin`, `cos`, `exp`, `abs`, etc.) but no other free variables.
- The output is a one-sided spectrum from DC (`freq=0`) up to the
  Nyquist frequency (`fs/2`), where `fs = 1/dt`.
- Aliasing/leakage follow standard DFT behavior: increase `k` (more
  samples) for finer frequency resolution, and make sure `fs = 1/dt` is
  at least twice the highest frequency component you expect to resolve.
