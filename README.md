# Similimum finder

A free, static website that suggests remedies from symptoms. You can **tap a body part and pick symptoms** (about 140 ready-made, labelled in English, Hindi and Marathi), or **type in your own words** in English, Hindi, Marathi or Hinglish. The whole interface switches between **English, हिंदी and मराठी**. For every case it gives three things:

- **Homeopathic dilution**: the best-matching remedy, with potency guidance (30C, 200C, mother tincture Q where Boericke uses it).
- **Biochemic tissue salt**: the closest of Schüssler's 12 salts (6X) for the same symptoms.
- **Bach flower remedy**: up to three of Dr Bach's 38 remedies for how you feel, plus the crisis formula (Rescue Remedy) for shock, panic or exam nerves.

It works the way a homeopath repertorises a case. Your words are turned into repertory language, split into separate symptoms, and each symptom is matched to the closest rubric in a Kent-style repertory. Boericke's Materia Medica is searched alongside it. Remedies are then ranked by how many of your symptoms they cover and how strongly they are graded. Follow-up questions (thirst, hot or chilly, mood, what made it start) are picked automatically to separate the top remedies.

> This is a learning and testing tool, not medical advice. For anything serious or long-lasting, for babies, or during pregnancy, see a doctor or a qualified homeopath. The site shows emergency warnings for things like chest pain, breathing trouble or bleeding.

## What's new: Common problems, offline app, notes from your own books

- **Common problems tab**: 51 everyday problems (fever, cold, cough, acidity, loose motions, piles, period pain, teething, backache, insomnia and more) in English, Hindi and Marathi. Each shows the remedies the classic books recommend for it, with the book's own advice for when each fits, and a **Find which one fits me** button that carries the problem into your case. Built from W. A. Dewey, *Practical Homoeopathic Therapeutics* (1900, public domain); five problems whose scanned chapter could not be read use Boericke's Repertory or Kent instead.
- **Smarter matching**: when you type a problem by name (fever, बवासीर, jukam…), Dewey's list for it is added at a light weight. On the 95 typed test cases this lifted first-place hits from 76 to 79; top-three stayed at 87.
- **Save to my notes** on every result, and a tip on the My notes tab for adding advice from a home book such as *धन्वंतरी तुमच्या घरी* in your own words. That book is still under copyright (K. B. K. Garde, Nirali Prakashan), so its text is not copied into the app; your notes stay in your browser and count when your symptoms match.
- **Installable and offline** (GitHub Pages only): an **Install app** button appears in supported browsers, and the app works without internet after the first visit (`manifest.webmanifest`, `sw.js`, `icons/`).

Rebuild the conditions data with `python3 tools/build_books.py /path/to/alternative-care-vault/processed/homeopathy` (source: [github.com/Cipher-Text/alternative-care-vault](https://github.com/Cipher-Text/alternative-care-vault), public-domain books only), then `python3 tools/build.py`.

## Put it on GitHub Pages (5 minutes)

1. Create a new repository on GitHub, for example `similimum-finder`.
2. Upload everything in this folder, keeping the folders as they are (`index.html`, `js/`, `data/`, `tests/`, `README.md`, `LICENSE`, `.nojekyll`).
3. Open **Settings → Pages**. Under "Build and deployment", choose **Deploy from a branch**, pick `main` and `/ (root)`, and save.
4. After a minute your site is live at `https://<your-username>.github.io/similimum-finder/`.

To try it on your own computer, just double-click `index.html`. It works offline.

## What is inside

| Path | What it is |
|---|---|
| `index.html` | The page and its styling |
| `js/similimum.js` | **Built file** the page loads (all of the scripts below bundled together) |
| `data/data.js` | **Built file** the page loads (the three JSON data files bundled together) |
| `tools/build.py` | Rebuilds the two built files. Run `python3 tools/build.py` after editing anything in `js/` or `data/*.json` |
| `js/app.js` | Screen logic: input, results, questions, "what I understood", biochemic and Bach cards |
| `js/bach.js` | The 38 Bach flower remedies, crisis formula, and the feeling words that select them |
| `js/engine.js` | The matching and ranking engine, safety checks, follow-up questions |
| `js/lexicon.js` | The language layer: Hindi, Marathi, Hinglish and everyday English words mapped to repertory terms. **Add your own words here.** |
| `data/repertory.json` | 74,482 rubrics with remedy grades 1–3 (Repertorium Publicum) |
| `data/boericke.json` | Boericke's Pocket Manual of Homoeopathic Materia Medica, 686 remedies |
| `data/remedies.json` | Remedy names and abbreviations |
| `data/boericke-repertory.json` | Boericke's clinical Repertory (1927): 5,368 rubrics by condition (see licence note below) |
| `js/quick.js` | The body parts and quick symptoms, in three languages, each pinned to checked rubrics |
| `js/i18n.js` | All interface text in English, Hindi and Marathi, plus Bach descriptions and safety messages |
| `tests/` | 75 textbook cases and a test runner |

## How accurate is it?

It is checked against 111 cases: 95 typed cases and 16 cases built only by tapping quick symptoms. Of the typed cases, it was checked against 75 classic "keynote" cases from homeopathic teaching (Belladonna fever, Bryonia headache, Pulsatilla cold, Arnica injury and so on), 15 of them written in Hindi, Marathi or Hinglish.

- The first 40 cases were used while building it. The other 35 were written later and first run blind.
- On those 35 unseen cases, the first run put the textbook remedy **first about half the time**, in the **top three about three times in four**, and in the **top five about four times in five**.
- After filling vocabulary gaps those cases exposed, the full set of 75 now scores 75% first, 92% top three, 95% top five. That later figure is flattering, because some of the fixes came from looking at those cases.

Accuracy goes up a lot when you answer the follow-up questions, untick any rubric in "What I understood" that is wrong, and star (★) the symptoms that are strongest and clearest.

A further 20 everyday cases (food poisoning, acidity, tonsillitis, period cramps, mosquito bite, some in Hindi, Marathi and Hinglish) were written before Boericke's Repertory was added. On their first run, the new layer moved first-place hits from 10 to 11 of 20 and top-three hits from 14 to 15. Cases built from tapped quick symptoms score 12 of 16 first and 14 of 16 in the top three.

Run the tests yourself (needs Node.js 18 or newer):

```bash
node tests/run-tests.mjs          # 40 building cases
node tests/run-tests.mjs --blind  # the last 15 cases
node tests/run-tests.mjs --all -v # all 95 typed cases, with details for misses
node tests/run-chips.mjs          # the 16 tap-only cases
```

## Improving it

After any change in `js/` or `data/`, run `python3 tools/build.py` so the page picks it up.

- **Add words**: open `js/lexicon.js` and add pairs like `['पित्त', ' heartburn ']` or `['sardi', 'coryza']`. Longer phrases are matched first.
- **Add test cases**: add `['Remedy-abbrev.', 'symptoms']` lines to `tests/cases.mjs` and rerun the tests to see the effect.
- **Change follow-up questions**: edit `QUESTIONS` in `js/engine.js`. Each answer points to rubric ids from `data/repertory.json`.

## Sources and licence

- **Repertory**: *Repertorium Publicum*, Vladimir Polony (2008), released under GPL v3 as part of OpenRep.
- **Materia Medica**: William Boericke, *Pocket Manual of Homoeopathic Materia Medica* (1906), public domain.
- **Bach flowers**: Dr Edward Bach's 38 remedies and crisis formula, as listed by the [Bach Centre](https://bachcentre.com/en/remedies/the-38-remedies/quick-reference-guide/); descriptions are written in plain words for this tool.
- **Clinical repertory**: Oscar E. Boericke, *Repertory* (1927), digital edition compiled by **Dr. Sohil Momin**, [github.com/drsamonline/boericke-repertory](https://github.com/drsamonline/boericke-repertory). Its licence allows free **non-commercial** use with credit to Dr. Sohil Momin and a link to that repository. If this site is ever used commercially (for example to sell remedies), delete `data/boericke-repertory.json`, run `python3 tools/build.py`, and the site works without it.
- The repertory and Boericke were taken from the open-source [OOREP project](https://github.com/nondeterministic/oorep) (GPL v3).

Because the repertory data is GPL v3, this project is released under the GNU General Public License v3 as well (see `LICENSE`).
