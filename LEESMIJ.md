# @radicle/thema

Het gedeelde ontwerpsysteem van Radicle, als één CSS-bestand.

## Waarom dit bestaat

Radicle-website, Discendo en Periscope hadden op 9 september 2026 samen 1107
regels thema en deelden daarvan niets. Gemeten:

| Repo | Tailwind | regels in globals.css | tokens |
|---|---|---|---|
| discendo | ^4.2.2 | 337 | 119 |
| radicle-website | ^4.2.2 | 586 | 198 |
| periscope | ^4 | 184 | 92 |

Zeventig tokennamen komen in alle drie voor, want alle drie gebruiken shadcn.
**Van die zeventig heeft er geen enkele in alle drie dezelfde waarde.** Een
steekproef:

| Token | discendo | radicle-website | periscope |
|---|---|---|---|
| `--radius` | 6px | 0.75rem | 0.875rem |
| `--background` | `oklch(0.9851 0 0)` | `oklch(0.9925 0.0040 90)` | `oklch(1 0 0)` |
| `--foreground` | `oklch(0.2046 0 0)` | `oklch(0.2350 0.0450 264)` | `oklch(0.1450 0 0)` |

Er viel dus niets te extraheren. Er viel iets te besluiten.

## Wat er in zit, en wat niet

**Het systeem**, hier: de neutrale ladder, de radiusladder en de letterafstand
op lopende tekst.

**De identiteit**, per product: de accentkleur en het lettertype.

**Schaduwen ook per product**, en dat was op 9 september 2026 nog niet zo. Ze
stonden hier, en Discendo bleek onder dezelfde naam `--shadow-ring` een andere
waarde te hebben staan: hun rand is een ring van 1px zonder inset, gemeten op de
Vercel-app, en die van elevenlabs.io is een halve pixel mét inset. Het product
importeert dit bestand en schrijft er daarna overheen, dus die van het product won
zonder dat iemand het zag. Eén naam die per repo iets anders betekent is erger dan
geen naam, dus zijn ze eruit tot een tweede repo ze echt nodig heeft.

Die scheiding is een keuze van Wilfred, 9 september 2026: "die kan per org anders
inderdaad". Drie producten die er precies hetzelfde uitzien betekent dat geen van
drieën een gezicht heeft.

Het lettertype hoort er om nog een reden buiten te blijven. Circular XX is
gekocht voor **www.getradicle.nl en dat domein alleen**, in de sneden Medium en
Bold. Een `@font-face` in dit pakket zou hem binnen een week op drie domeinen
zetten en dat valt buiten de licentie.

## Alles is gemeten

De bron is elevenlabs.io, doorgemeten op 8 en 9 september 2026. In `thema.css`
staat per blok de meting erbij, zodat een volgende sessie hem kan narekenen.

De scherpste: hun gedempte tekst `rgb(119,113,105)` is `oklch(55.2% 0.014 75.3)`
en Tailwinds stone-500 is `oklch(55.3% 0.013 58.071)`. Ze draaien op dezelfde
ladder.

## Gebruik

```json
"@radicle/thema": "github:radicle-ventures/thema#v2"
```

Geen registry, geen tokens in CI, geen buildstap. In de `globals.css` van het
product:

```css
@import "tailwindcss";
@import "@radicle/thema";

:root {
  --primary: /* de accentkleur van dit product */;
  --border: /* hangt samen met de achtergrond die dit product kiest */;
}
```

Vereist Tailwind 4, want het thema is een CSS-bestand met `@theme`.

## Versies

Vastgezet op een git-tag, zodat een wijziging aan het thema niet vanzelf
doorslaat in drie producten tegelijk. Je bumpt per repo wanneer het uitkomt.

## Wat adoptie kost

Dit pakket invoeren verandert hoe een product eruitziet, want er is nu niets
gedeeld. Reken erop dat de radius en de neutralen zichtbaar verschuiven. Doe het
per repo, kijk zelf naar de preview en merge pas daarna.

Discendo is de eerste: de neutrale ladder en de letterafstand staan daar sinds
PR #145 al zo, dus die kan hier zonder zichtbare wijziging op over. De radius
gaat daar wel van 6px naar 4px.
