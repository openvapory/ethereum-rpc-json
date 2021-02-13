# vapory-rpc-json

JSON file of all vapory's rpc methods supported by tetcoin

## interfaces

[interfaces.md](interfaces.md) contains the auto-generated list of interfaces exposed, along with their relevant documentation

## contributing

0. Clone the repo
0. Branch
0. Add the missing interfaces only into `lib/interfaces/*.js`
0. Parameters (array) & Returns take objects of type
    - `{ type: [Array|Boolean|Object|String|...], desc: 'some description' }`
    - Types are built-in JS types or those defined in `lib/types.js` (e.g. `BlockNumber`, `Quantity`, etc.)
    - If a formatter is required, add it as `format: 'string-type'`
0. Run the lint & tests, `npm run lint && npm run testOnce`
0. Generate via `npm run build` which outputs `index.js`, `index.json` & `interfaces.md` (Only required until Travis is fully in-place)
0. Check-in and make a PR
