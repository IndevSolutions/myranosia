---
name: Life
---
# 
The domain of living things - their growth, their healing, and the vital spark that animates all of Myria's creation. Every creature that draws breath carries within it a fragment of the Mother of All, and it is Life's gods who tend that fragment, nurture it, and fight to preserve it against the endless patience of Nos. From the first breath of a newborn to the recovery of a wounded soldier, from the flourishing of a forest to the persistence of hope in desperate circumstances, Life governs everything that chooses, however stubbornly, to continue. It is the oldest argument against entropy, and it has never once conceded defeat.

![[Life.png]]

---
<div class="page-break" style="page-break-before: always;"></div>

```dataviewjs

const domainName = dv.current().name;

  

function domainMatch(p) {

  if (!p.domain) return false;

  if (Array.isArray(p.domain)) return p.domain.includes(domainName);

  return p.domain === domainName;

}

  

function getMoniker(p) {

  if (!p.moniker) return "";

  if (Array.isArray(p.moniker)) return p.moniker.join(" / ");

  return p.moniker;

}

  

const primePages = dv.pages('"Mythos/Pantheon"').where(p => domainMatch(p) && p.tier === "Prime").array();

const manifestPages = dv.pages('"Mythos/Pantheon"').where(p => domainMatch(p) && p.tier === "Manifestation").sort(p => ["Lawful Good","Neutral Good","Chaotic Good","Lawful Neutral","True Neutral","Chaotic Neutral","Lawful Evil","Neutral Evil","Chaotic Evil"].indexOf(p.alignment)).array();

const lesserPages = dv.pages('"Mythos/Pantheon"').where(p => domainMatch(p) && p.tier === "Lesser").sort(p => p.alignment).array();

  

function chunkArray(arr, size) {

  const chunks = [];

  for (let i = 0; i < arr.length; i += size) chunks.push(arr.slice(i, i + size));

  return chunks;

}

  

function buildCard(p) {

  const imgPath = p.image ? app.vault.adapter.getResourcePath(p.image) : null;

  return '<div style="background:var(--background-secondary); border:1px solid var(--background-modifier-border); border-radius:10px; overflow:hidden; width:160px; flex-shrink:0; box-shadow: 0 2px 6px rgba(0,0,0,0.15);">' +

    (imgPath ? '<img src="' + imgPath + '" style="width:100%; height:240px; object-fit:cover;">' : '<div style="width:100%; height:240px; background:var(--background-modifier-border); display:flex; align-items:center; justify-content:center; color:var(--text-faint); font-size:0.75em;">No Image</div>') +

    '<div style="padding:8px 10px;">' +

      '<div style="font-size:0.9em; font-weight:700; color:var(--text-accent); border-bottom:1px solid var(--background-modifier-border); padding-bottom:4px; margin-bottom:4px;">' +

        '<a href="' + p.file.path + '" class="internal-link">' + p.name + '</a>' +

      '</div>' +

      '<div style="font-size:0.78em; color:var(--text-muted); font-style:italic;">' + getMoniker(p) + '</div>' +

    '</div>' +

  '</div>';

}

  

function buildCardRow(pages) {

  let row = '<div style="display:flex; flex-wrap:wrap; gap:12px; justify-content:center; padding:8px 0;">';

  for (let p of pages) row += buildCard(p);

  row += '</div>';

  return row;

}

  

function buildSection(label, chunks, emptyMsg) {

  const rowCount = Math.max(chunks.length, 1);

  let html = '';

  if (chunks.length > 0) {

    html += '<tr>';

    html += '<td style="vertical-align:middle; padding:16px 20px 16px 16px; font-weight:700; font-size:0.95em; color:var(--text-on-accent); background:var(--color-accent); border-radius:8px; white-space:nowrap; width:140px; text-align:center;" rowspan="' + rowCount + '">' + label + '</td>';

    html += '<td style="padding:4px 8px;">' + buildCardRow(chunks[0]) + '</td>';

    html += '</tr>';

    for (let i = 1; i < chunks.length; i++) {

      html += '<tr><td style="padding:4px 8px;">' + buildCardRow(chunks[i]) + '</td></tr>';

    }

  } else {

    html += '<tr>';

    html += '<td style="vertical-align:middle; padding:16px 20px 16px 16px; font-weight:700; font-size:0.95em; color:var(--text-on-accent); background:var(--color-accent); border-radius:8px; white-space:nowrap; width:140px; text-align:center;">' + label + '</td>';

    html += '<td style="padding:16px 8px; color:var(--text-muted); font-style:italic;">' + emptyMsg + '</td>';

    html += '</tr>';

  }

  return html;

}

  

const primeName = primePages.length > 0 ? primePages[0].name : domainName;

const primeChunks = chunkArray(primePages, 3);

const manifestChunks = chunkArray(manifestPages, 3);

const lesserChunks = chunkArray(lesserPages, 3);

  

let html = '<table style="width:100%; border-collapse:separate; border-spacing:0 8px;">';

html += buildSection("Prime<br>Deity", primeChunks, "No prime deity created yet.");

html += '<tr><td colspan="2" style="padding:4px 0;"></td></tr>';

html += buildSection("Manifestations<br>of " + primeName, manifestChunks, "No manifestations created yet.");

html += '<tr><td colspan="2" style="padding:4px 0;"></td></tr>';

html += buildSection("Lesser<br>Deities", lesserChunks, "No lesser deities created yet.");

html += '</table>';

  

dv.el("div", "", {attr: {style: "width:100%;"}}).innerHTML = html;

```