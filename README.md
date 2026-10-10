# Closing Rank

A free, static website that shows NEET PG closing ranks for every college, course, quota and
seat category in MCC (All India) counselling. Enter a rank and category to see which seats
that rank would have been allotted.

All numbers are counted from the final allotment PDFs that the Medical Counselling Committee
publishes at https://mcc.nic.in/archive-pg/. State counselling, fees, stipends and bond terms
are not included.

## Layout

- `site/` is the whole website: `index.html` and `data.json`. No build step, no server code.
- `scripts/extract.py` reads the MCC PDFs in `data/raw/` into `data/parsed/`.
- `scripts/build.py` cleans the parsed rows and writes `site/data.json`.

## View it locally

    cd site
    python -m http.server 8765

Then open http://127.0.0.1:8765/. Opening `index.html` directly as a file will not load the data.

## Put it online for free

Upload the `site/` folder to any static host. Two that need no configuration:

- Netlify Drop: drag the `site` folder onto https://app.netlify.com/drop
- GitHub Pages: push the repository and set Pages to serve the `site` folder
  (or copy its two files to the repository root)

## Add a new round or year

1. Find the "Final Result" PDF for the round on https://mcc.nic.in/archive-pg/ (or the current
   PG counselling page) and save it in `data/raw/` as `<year>_<round>.pdf`, where round is
   `r1`, `r2`, `r3` or `stray`, for example `2026_r1.pdf`.
2. Run:

       .venv\Scripts\python.exe -I scripts\extract.py data\raw data\parsed
       .venv\Scripts\python.exe -I scripts\build.py data\parsed site\data.json

   `extract.py` skips PDFs it has already parsed. The first setup needs
   `python -m venv .venv` and `.venv\Scripts\python.exe -m pip install pymupdf`.
3. Upload the new `site/data.json`.
