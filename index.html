import { useState, useCallback } from 'react';

const C = {
  ink: '#0d0d0e', concrete: '#181820', slab: '#22222e', crack: '#2e2e3e',
  dust: '#52526a', smoke: '#8888a0', white: '#f0eff4',
  magenta: '#e8006e', volt: '#b8ff00', indigo: '#5533ff',
};

function parseCSV(text) {
  text = text.replace(/^\uFEFF/, '');
  const lines = text.trim().split(/\r?\n/);
  if (lines.length < 2) return [];
  const sep = lines[0].split('\t').length > lines[0].split(',').length ? '\t' : ',';
  const headers = lines[0].split(sep).map(h => h.trim().replace(/^"|"$/g, '').toLowerCase());
  return lines.slice(1).filter(l => l.trim()).map(line => {
    const cols = []; let cur = '', inQ = false;
    for (let i = 0; i < line.length; i++) {
      const ch = line[i];
      if (ch === '"') { inQ = !inQ; continue; }
      if (ch === sep && !inQ) { cols.push(cur.trim()); cur = ''; continue; }
      cur += ch;
    }
    cols.push(cur.trim());
    const obj = {};
    headers.forEach((h, i) => { obj[h] = (cols[i] || '').replace(/^"|"$/g, '').trim(); });
    return obj;
  });
}

function normalise(row, type) {
  const keys = Object.keys(row);
  const find = (...cands) => {
    const k = keys.find(k => {
      const kn = k.replace(/[\s_\-]/g, '').toLowerCase();
      return cands.some(c => kn.includes(c));
    });
    return k ? row[k] : '';
  };
  const keyword = (find('keyword', 'query', 'term') || Object.values(row)[0] || '').toLowerCase().trim();
  const url = type === 'comp' ? 'N/A' : (find('currenturl', 'url', 'page') || 'N/A');
  const svRaw = find('volume', 'sv', 'searchvol') || find('organictraffic', 'traffic') || '0';
  const sv = parseInt(svRaw.replace(/[^0-9]/g, ''), 10) || 0;
  const location = find('location', 'countrycode', 'country') || '';
  return { keyword, url, sv, source: type === 'comp' ? 'Competitor' : 'Client', location };
}

function extractBrand(siteUrl) {
  if (!siteUrl?.trim()) return null;
  try {
    const url = new URL(siteUrl.trim().startsWith('http') ? siteUrl.trim() : 'https://' + siteUrl.trim());
    const hostname = url.hostname.replace(/^www\./, '');
    const parts = hostname.split('.');
    return parts.length >= 2 ? parts[parts.length - 2].toLowerCase() : parts[0].toLowerCase();
  } catch (e) { return null; }
}

function groupKeywords(keywords, collUrls, siteUrl) {
  const brand = extractBrand(siteUrl);
  console.log('Brand:', brand, 'Keywords:', keywords.length);

  const rules = [
    { name: 'Branded Keywords', priority: 200, match: kw => brand && new RegExp('\\b' + brand + '\\b', 'i').test(kw) },
    
    // Location-based (high priority for service businesses)
    { 
      name: 'Location-Based Keywords', 
      priority: 180, 
      match: kw => {
        // Australian cities and regions
        const locations = /\b(sydney|melbourne|brisbane|perth|adelaide|canberra|hobart|darwin|gold coast|newcastle|wollongong|geelong|townsville|cairns|toowoomba|ballarat|bendigo|albury|launceston|mackay|rockhampton|bunbury|bundaberg|hervey bay|wagga wagga|coffs harbour|shepparton|port macquarie|tamworth|orange|dubbo|nsw|vic|qld|wa|sa|tas|nt|act|australia|australian)\b/i;
        const suburbs = /\b(parramatta|bondi|manly|surry hills|paddington|fitzroy|st kilda|south yarra|richmond|fortitude valley|new farm|west end|fremantle|subiaco|glenelg|north adelaide)\b/i;
        const modifiers = /\bnear me\b|\bin\s|\bat\s|local\s/i;
        
        return locations.test(kw) || suburbs.test(kw) || modifiers.test(kw);
      }
    },
    
    { name: 'Informational Intent', priority: 150, match: kw => /^(how|what|where|why|when|which|who|if|can|should|is|are|do|does)\s/i.test(kw) || /\b(how|what|why|where|if)\s/i.test(kw) },
    { name: 'Size & Capacity Info', priority: 140, match: kw => /\d+\s?(oz|ml)\s+(in|to)\s+(ml|oz)|size\s+chart|capacity|how\smany\sml/i.test(kw) },
    { name: 'Reusable Coffee Cups', priority: 100, match: kw => /\breusabl|\beco\b|sustainabl/i.test(kw) && /coffee|latte/i.test(kw) && /\bcup\b|\bcups\b|mug/i.test(kw) && !/\btravel\b|\binsulat|\bthermal\b|takeaway|tumbler|bottle/i.test(kw) },
    { name: 'Travel Mugs', priority: 98, match: kw => /\btravel\b/i.test(kw) && /\bmug\b|\bmugs\b/i.test(kw) && !/tumbler/i.test(kw) },
    { name: 'Tumblers', priority: 97, match: kw => /tumbler/i.test(kw) && /coffee|iced|latte/i.test(kw) },
    { name: 'Insulated / Thermal Cups', priority: 96, match: kw => /\binsulat|\bthermal\b|\bthermos\b|vacuum/i.test(kw) && /\bcup\b|\bmug\b|tumbler/i.test(kw) && !/^travel\s(mug|cup)/i.test(kw) },
    { name: 'Stainless Steel Cups', priority: 94, match: kw => /\bstainless\b|\bsteel\b|\bmetal\b/i.test(kw) && /\bcup\b|\bmug\b/i.test(kw) && !/insulat|thermal/i.test(kw) },
    { name: 'Iced Coffee Cups', priority: 95, match: kw => /\biced\scoffee\b|\bcold\scoffee\b/i.test(kw) && /\bcup\b|tumbler/i.test(kw) },
    { name: 'Takeaway / Disposable Cups', priority: 93, match: kw => /takeaway|take\saway|disposab|plastic\scup|paper\scup|supplier.*cup/i.test(kw) },
    { name: 'Custom / Personalised Cups', priority: 92, match: kw => /\bcustom\b|personalise|personaliz|branded|corporate|logo|print/i.test(kw) && /\bcup\b|\bmug\b|tumbler/i.test(kw) },
    { name: 'Glass Coffee Cups', priority: 90, match: kw => /\bglass\b/i.test(kw) && /\bcup\b|\bmug\b/i.test(kw) && !/insulat|thermal/i.test(kw) },
    { name: 'Ceramic Coffee Cups', priority: 89, match: kw => /\bceramic\b|porcelain/i.test(kw) && /\bcup\b|\bmug\b/i.test(kw) },
    { name: 'Aesthetic / Design Cups', priority: 88, match: kw => /aesthetic|cute|cool|pretty|stylish/i.test(kw) && /\bcup\b|\bmug\b/i.test(kw) && !/reusabl|travel|insulat|takeaway|glass|ceramic/i.test(kw) },
    { name: 'Specific Coffee Types', priority: 85, match: kw => /\bpiccolo\b|\bespresso\scup|\bbabycino\b|\bcamping\s(mug|cup)|\bbarista\scup/i.test(kw) },
    { name: 'Kids & Baby Cups', priority: 87, match: kw => /\b(kids|baby|children|toddler)\b/i.test(kw) && /\bcup\b|\bmug\b|bottle|sippy/i.test(kw) },
    { name: 'Straws & Accessories', priority: 80, match: kw => (/^(straw|lid|sleeve)/i.test(kw) || /\b(straw|lid|sleeve)\b/i.test(kw) || /boba|bubble\stea/i.test(kw)) && !/\b(cup|mug)\s+(with|and)\s+(lid|straw)/i.test(kw) },
    { name: 'Water Bottles', priority: 75, match: kw => /\bbottle\b|\bflask\b/i.test(kw) && (/\bwater\b|\bdrink\b|hydrat/i.test(kw) || !/coffee|latte|iced\scoffee/i.test(kw)) },
  ];

  const groups = {};
  rules.forEach(r => { groups[r.name] = []; });
  groups['Other / Uncategorised'] = [];

  keywords.forEach(kw => {
    let matched = false;
    for (const rule of rules) {
      try {
        if (rule.match(kw.keyword)) {
          groups[rule.name].push(kw);
          matched = true;
          break;
        }
      } catch (e) {}
    }
    if (!matched) groups['Other / Uncategorised'].push(kw);
  });

  return Object.entries(groups)
    .filter(([n, r]) => r.length > 0)
    .map(([n, r]) => ({ label: n, slug: n.toLowerCase().replace(/[^a-z0-9]+/g, '-'), rows: r }))
    .sort((a, b) => {
      if (a.slug.includes('branded')) return -1;
      if (b.slug.includes('branded')) return 1;
      if (a.slug.includes('informational')) return -1;
      if (b.slug.includes('informational')) return 1;
      if (a.slug.includes('other')) return 1;
      if (b.slug.includes('other')) return -1;
      return b.rows.length - a.rows.length;
    });
}

export default function App() {
  const [tab, setTab] = useState(0);
  const [siteUrl, setSiteUrl] = useState('');
  const [collections, setCollections] = useState('');
  const [clientRows, setClientRows] = useState([]);
  const [compRows, setCompRows] = useState([]);
  const [status, setStatus] = useState({ type: '', msg: '' });
  const [groups, setGroups] = useState([]);
  const [expanded, setExpanded] = useState(null);
  const [filterSrc, setFilterSrc] = useState('all');
  const [searchQ, setSearchQ] = useState('');

  const handleFile = useCallback((file, type) => {
    if (!file) return;
    const sniffer = new FileReader();
    sniffer.onload = (e) => {
      const bytes = new Uint8Array(e.target.result);
      const isUtf16 = (bytes[0] === 0xFF && bytes[1] === 0xFE) || (bytes[0] === 0xFE && bytes[1] === 0xFF);
      const reader = new FileReader();
      reader.onload = (ev) => {
        try {
          const parsed = parseCSV(ev.target.result).map(r => normalise(r, type)).filter(r => r.keyword?.length > 0);
          if (type === 'client') {
            setClientRows(parsed);
            setStatus({ type: 'ok', msg: parsed.length + ' keywords loaded' });
          } else {
            setCompRows(parsed);
            setStatus({ type: 'ok', msg: parsed.length + ' keywords loaded' });
          }
        } catch (err) {
          setStatus({ type: 'err', msg: 'Parse error: ' + err.message });
        }
      };
      reader.readAsText(file, isUtf16 ? 'utf-16' : 'utf-8');
    };
    sniffer.readAsArrayBuffer(file.slice(0, 4));
  }, []);

  const runGrouping = useCallback(() => {
    const allKws = [...clientRows, ...compRows];
    if (!allKws.length) {
      setStatus({ type: 'err', msg: 'Upload CSV first' });
      return;
    }
    const collUrls = collections.trim() ? collections.split('\n').map(l => l.trim()).filter(Boolean) : [];
    const result = groupKeywords(allKws, collUrls, siteUrl);
    setGroups(result);
    setExpanded(result[0]?.slug || null);
    setTab(4);
    setStatus({ type: 'ok', msg: result.length + ' groups created' });
  }, [clientRows, compRows, collections, siteUrl]);

  const doExport = useCallback(() => {
    try {
      if (!groups?.length) return setStatus({ type: 'err', msg: 'No data' });
      const rows = [['Group', 'Keyword', 'URL', 'Volume', 'Source']];
      groups.forEach(g => g.rows?.forEach(r => rows.push([g.label ?? '', r.keyword ?? '', r.url ?? '', r.sv ?? 0, r.source ?? ''])));
      const csv = '\uFEFF' + rows.map(r => r.map(c => '"' + String(c).replace(/"/g, '""') + '"').join(',')).join('\r\n');
      const blob = new Blob([csv], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url;
      a.download = 'calibre9-groups.csv';
      document.body.appendChild(a);
      a.click();
      setTimeout(() => { document.body.removeChild(a); URL.revokeObjectURL(url); }, 100);
      setStatus({ type: 'ok', msg: 'Downloaded' });
    } catch (err) {
      setStatus({ type: 'err', msg: 'Export failed' });
    }
  }, [groups]);

  const resetAll = useCallback(() => {
    if (confirm('Reset all data? This will clear your URL, keywords, collections, and groups.')) {
      setSiteUrl('');
      setCollections('');
      setClientRows([]);
      setCompRows([]);
      setGroups([]);
      setExpanded(null);
      setFilterSrc('all');
      setSearchQ('');
      setStatus({ type: '', msg: '' });
      setTab(0);
    }
  }, []);

  const filteredGroups = groups.map(g => ({ ...g, rows: g.rows.filter(r => (filterSrc === 'all' || r.source.toLowerCase() === filterSrc) && (!searchQ || r.keyword.includes(searchQ.toLowerCase()))) })).filter(g => g.rows.length > 0);
  const totalSV = groups.reduce((s, g) => s + g.rows.reduce((ss, r) => ss + r.sv, 0), 0);
  const totalKws = groups.reduce((s, g) => s + g.rows.length, 0);

  return (
    <div style={{ minHeight: '100vh', background: C.ink, color: C.white, fontFamily: "'Montserrat',sans-serif" }}>
      <style>{`@import url('https://fonts.googleapis.com/css2?family=Montserrat:wght@400;700;900&display=swap');*{margin:0;padding:0;box-sizing:border-box}@keyframes fadeUp{from{opacity:0;transform:translateY(10px)}to{opacity:1;transform:none}}@keyframes flicker{0%,100%{opacity:1}93%{opacity:.75}}.head{font-family:'Acumin Pro ExtraCondensed','Arial Narrow',sans-serif;font-weight:900;text-transform:uppercase;letter-spacing:0.05em}.upzone:hover{border-color:${C.magenta};background:#e8006e0d}input::placeholder{color:${C.dust}}::-webkit-scrollbar{width:5px}::-webkit-scrollbar-track{background:${C.concrete}}::-webkit-scrollbar-thumb{background:${C.crack}}`}</style>

      <div style={{ background: C.concrete, borderBottom: '1px solid ' + C.crack, position: 'sticky', top: 0, zIndex: 500 }}>
        <div style={{ padding: '0 36px', height: 62, display: 'flex', alignItems: 'center', justifyContent: 'space-between', borderBottom: '1px solid ' + C.crack }}>
          <div style={{ display: 'flex', alignItems: 'center', gap: 16 }}>
            <div style={{ width: 44, height: 44, background: C.magenta, display: 'flex', alignItems: 'center', justifyContent: 'center', fontSize: 21, color: '#fff', clipPath: 'polygon(10% 0%,100% 0%,90% 100%,0% 100%)', animation: 'flicker 9s infinite', fontWeight: 900 }}>C9</div>
            <div>
              <div className="head" style={{ fontSize: 28, lineHeight: 1 }}>CALIBRE<span style={{ color: C.magenta }}>9</span></div>
              <div style={{ fontSize: 11, color: C.dust, marginTop: 1 }}>KEYWORD INTELLIGENCE</div>
            </div>
          </div>
          <div style={{ display: 'flex', gap: 10 }}>
            {groups.length > 0 && (
              <button onClick={doExport} style={{ background: C.volt, color: C.ink, border: 'none', padding: '9px 20px', fontWeight: 900, fontSize: 10, cursor: 'pointer', display: 'flex', alignItems: 'center', gap: 7 }}>
                <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" strokeWidth="2.5"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4M7 10l5 5 5-5M12 15V3" /></svg>
                Export
              </button>
            )}
            <button onClick={resetAll} style={{ background: 'none', border: '1px solid ' + C.crack, color: C.dust, padding: '9px 16px', fontWeight: 700, fontSize: 10, cursor: 'pointer', letterSpacing: '0.06em', textTransform: 'uppercase', transition: 'all 0.15s' }} onMouseEnter={e => { e.currentTarget.style.borderColor = C.smoke; e.currentTarget.style.color = C.smoke; }} onMouseLeave={e => { e.currentTarget.style.borderColor = C.crack; e.currentTarget.style.color = C.dust; }}>
              Reset
            </button>
          </div>
        </div>

        <div style={{ display: 'flex', padding: '0 36px', gap: 2 }}>
          {['Website', 'Client', 'Competitor', 'Collections', 'Groups'].map((l, i) => (
            <button key={i} onClick={() => setTab(i)} style={{ background: 'none', border: 'none', borderBottom: tab === i ? '3px solid ' + C.magenta : '3px solid transparent', color: tab === i ? C.white : C.dust, padding: '14px 20px', fontWeight: 700, fontSize: 11, cursor: 'pointer' }}>
              {l}
              {i === 1 && clientRows.length > 0 && <span style={{ background: C.magenta, color: '#fff', padding: '1px 6px', fontSize: 9, marginLeft: 7, borderRadius: 2 }}>{clientRows.length}</span>}
              {i === 2 && compRows.length > 0 && <span style={{ background: C.magenta, color: '#fff', padding: '1px 6px', fontSize: 9, marginLeft: 7, borderRadius: 2 }}>{compRows.length}</span>}
              {i === 4 && groups.length > 0 && <span style={{ background: C.magenta, color: '#fff', padding: '1px 6px', fontSize: 9, marginLeft: 7, borderRadius: 2 }}>{groups.length}</span>}
            </button>
          ))}
        </div>
      </div>

      {status.msg && (
        <div style={{ background: status.type === 'err' ? C.magenta + '15' : status.type === 'info' ? C.indigo + '12' : C.volt + '0d', padding: '10px 36px', fontSize: 11, color: status.type === 'err' ? C.magenta : status.type === 'info' ? C.indigo : C.volt, fontWeight: 500 }}>
          <span style={{ fontWeight: 900, marginRight: 4 }}>{status.type === 'err' ? 'ERR //' : status.type === 'info' ? '//' : 'OK //'}</span>{status.msg}
        </div>
      )}

      <div style={{ maxWidth: 1160, margin: '0 auto', padding: '38px 24px 100px' }}>

        {tab === 0 && (
          <div style={{ animation: 'fadeUp 0.3s' }}>
            <h1 className="head" style={{ fontSize: 64, lineHeight: 0.92, marginBottom: 16, borderLeft: '4px solid ' + C.magenta, paddingLeft: 20 }}>KEYWORD<br /><span style={{ color: C.magenta }}>INTELLIGENCE</span></h1>
            <p style={{ fontSize: 13, color: C.smoke, lineHeight: 1.75, maxWidth: 500, marginBottom: 32 }}>Intent-based grouping with branded & informational detection</p>
            <div style={{ background: C.concrete, border: '1px solid ' + C.crack, padding: 28, marginBottom: 24 }}>
              <div style={{ fontSize: 13, color: C.dust, marginBottom: 12 }}>Client Website URL</div>
              <input value={siteUrl} onChange={e => setSiteUrl(e.target.value)} placeholder="https://au.keepcup.com" style={{ width: '100%', background: C.ink, border: '1px solid ' + C.crack, color: C.white, fontSize: 13, padding: '12px 14px', outline: 'none' }} />
              <p style={{ fontSize: 11, color: C.dust, marginTop: 9 }}>Detects branded keywords automatically</p>
            </div>
            <button onClick={() => setTab(1)} style={{ background: C.magenta, color: '#fff', border: 'none', padding: '11px 28px', fontWeight: 900, fontSize: 11, cursor: 'pointer' }}>Start →</button>
          </div>
        )}

        {tab === 1 && (
          <div style={{ animation: 'fadeUp 0.3s' }}>
            <h2 className="head" style={{ fontSize: 44, marginBottom: 28 }}>CLIENT KEYWORDS</h2>
            <div className="upzone" onClick={() => document.getElementById('fc').click()} style={{ border: '2px dashed ' + C.crack, background: C.concrete, padding: 44, textAlign: 'center', cursor: 'pointer', marginBottom: 20 }}>
              <input type="file" id="fc" accept=".csv,.tsv,.txt" style={{ display: 'none' }} onChange={e => handleFile(e.target.files[0], 'client')} />
              {clientRows.length === 0 ? <><div style={{ marginBottom: 14 }}><svg width="38" height="38" viewBox="0 0 24 24" fill="none" stroke={C.dust} strokeWidth="1.2"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4M17 8l-5-5-5 5M12 3v12" /></svg></div><div className="head" style={{ fontSize: 20, color: C.smoke }}>DROP CSV</div></> : <><div style={{ fontSize: 34, color: C.volt }}>✓</div><div style={{ fontWeight: 700, fontSize: 13, color: C.volt }}>{clientRows.length} keywords</div></>}
            </div>
            <div style={{ display: 'flex', gap: 12 }}>
              <button onClick={() => setTab(0)} style={{ background: 'none', border: '1px solid ' + C.crack, color: C.dust, padding: '11px 22px', fontWeight: 700, fontSize: 11, cursor: 'pointer' }}>← Back</button>
              <button onClick={() => setTab(2)} style={{ background: C.magenta, color: '#fff', border: 'none', padding: '11px 28px', fontWeight: 900, fontSize: 11, cursor: 'pointer' }}>Competitor →</button>
            </div>
          </div>
        )}

        {tab === 2 && (
          <div style={{ animation: 'fadeUp 0.3s' }}>
            <h2 className="head" style={{ fontSize: 44, marginBottom: 28 }}>COMPETITOR</h2>
            <div className="upzone" onClick={() => document.getElementById('fcp').click()} style={{ border: '2px dashed ' + C.crack, background: C.concrete, padding: 44, textAlign: 'center', cursor: 'pointer', marginBottom: 20 }}>
              <input type="file" id="fcp" accept=".csv,.tsv,.txt" style={{ display: 'none' }} onChange={e => handleFile(e.target.files[0], 'comp')} />
              {compRows.length === 0 ? <><div style={{ marginBottom: 14 }}><svg width="38" height="38" viewBox="0 0 24 24" fill="none" stroke={C.dust} strokeWidth="1.2"><path d="M21 15v4a2 2 0 01-2 2H5a2 2 0 01-2-2v-4M17 8l-5-5-5 5M12 3v12" /></svg></div><div className="head" style={{ fontSize: 20, color: C.smoke }}>DROP CSV</div></> : <><div style={{ fontSize: 34, color: C.volt }}>✓</div><div style={{ fontWeight: 700, fontSize: 13, color: C.volt }}>{compRows.length} keywords</div></>}
            </div>
            <div style={{ display: 'flex', gap: 12 }}>
              <button onClick={() => setTab(1)} style={{ background: 'none', border: '1px solid ' + C.crack, color: C.dust, padding: '11px 22px', fontWeight: 700, fontSize: 11, cursor: 'pointer' }}>← Back</button>
              <button onClick={() => setTab(3)} style={{ background: C.magenta, color: '#fff', border: 'none', padding: '11px 28px', fontWeight: 900, fontSize: 11, cursor: 'pointer' }}>Collections →</button>
            </div>
          </div>
        )}

        {tab === 3 && (
          <div style={{ animation: 'fadeUp 0.3s' }}>
            <h2 className="head" style={{ fontSize: 44, marginBottom: 28 }}>COLLECTIONS</h2>
            <div style={{ background: C.concrete, border: '1px solid ' + C.crack, padding: 28, marginBottom: 20 }}>
              <textarea value={collections} onChange={e => setCollections(e.target.value)} rows={12} placeholder="https://au.keepcup.com/collections/..." style={{ width: '100%', background: C.ink, border: '1px solid ' + C.crack, color: C.white, fontSize: 12, padding: 14, outline: 'none', resize: 'vertical' }} />
            </div>
            <div style={{ display: 'flex', gap: 12 }}>
              <button onClick={() => setTab(2)} style={{ background: 'none', border: '1px solid ' + C.crack, color: C.dust, padding: '11px 22px', fontWeight: 700, fontSize: 11, cursor: 'pointer' }}>← Back</button>
              <button onClick={runGrouping} style={{ background: 'linear-gradient(108deg,' + C.magenta + ',' + C.indigo + ')', color: '#fff', border: 'none', padding: '11px 32px', fontWeight: 900, fontSize: 11, cursor: 'pointer' }}>⚡ Group</button>
            </div>
          </div>
        )}

        {tab === 4 && groups.length === 0 && (
          <div style={{ textAlign: 'center', padding: '100px 0' }}>
            <div className="head" style={{ fontSize: 44, color: C.smoke }}>NO DATA</div>
            <button onClick={() => setTab(0)} style={{ background: C.magenta, color: '#fff', border: 'none', padding: '11px 28px', fontWeight: 900, fontSize: 11, cursor: 'pointer', marginTop: 24 }}>Start →</button>
          </div>
        )}

        {tab === 4 && groups.length > 0 && (
          <div style={{ animation: 'fadeUp 0.3s' }}>
            <h2 className="head" style={{ fontSize: 38, marginBottom: 18 }}>RESULTS</h2>
            <div style={{ display: 'flex', gap: 10, flexWrap: 'wrap', marginBottom: 28 }}>
              {[{ l: 'Groups', v: groups.length }, { l: 'Keywords', v: totalKws }, { l: 'Total SV', v: totalSV }].map((s, i) => (
                <div key={i} style={{ flex: 1, minWidth: 110, background: C.slab, border: '1px solid ' + C.crack, borderTop: '3px solid ' + C.magenta, padding: '18px 16px' }}>
                  <div style={{ fontWeight: 700, fontSize: 9, color: C.dust, marginBottom: 7 }}>{s.l}</div>
                  <div className="head" style={{ fontSize: 28 }}>{s.v.toLocaleString()}</div>
                </div>
              ))}
            </div>
            <div style={{ display: 'flex', gap: 10, background: C.concrete, border: '1px solid ' + C.crack, padding: '12px 16px', marginBottom: 18, alignItems: 'center' }}>
              <input value={searchQ} onChange={e => setSearchQ(e.target.value)} placeholder="Search…" style={{ background: C.ink, border: '1px solid ' + C.crack, color: C.white, fontSize: 12, padding: '8px 14px', outline: 'none', width: 210 }} />
              {['all', 'client', 'competitor'].map(s => (
                <button key={s} onClick={() => setFilterSrc(s)} style={{ background: filterSrc === s ? (s === 'competitor' ? C.indigo : C.magenta) : 'none', border: '1px solid ' + (filterSrc === s ? 'transparent' : C.crack), color: filterSrc === s ? '#fff' : C.dust, padding: '7px 14px', fontWeight: 700, fontSize: 10, cursor: 'pointer' }}>
                  {s === 'all' ? 'All' : s}
                </button>
              ))}
              <div style={{ marginLeft: 'auto' }}>
                <button onClick={doExport} style={{ background: C.volt, color: C.ink, border: 'none', padding: '8px 18px', fontWeight: 900, fontSize: 10, cursor: 'pointer' }}>Export</button>
              </div>
            </div>
            <div style={{ display: 'flex', flexDirection: 'column', gap: 6 }}>
              {filteredGroups.map(g => {
                const open = g.slug === expanded;
                return (
                  <div key={g.slug} style={{ border: '1px solid ' + (open ? C.magenta + '55' : C.crack), background: C.concrete }}>
                    <div onClick={() => setExpanded(open ? null : g.slug)} style={{ padding: '14px 20px', display: 'flex', alignItems: 'center', gap: 13, cursor: 'pointer' }}>
                      <span style={{ fontFamily: 'monospace', fontSize: 9, color: open ? C.magenta : C.dust, transform: open ? 'rotate(90deg)' : 'none', display: 'inline-block' }}>▶</span>
                      <div style={{ width: 3, height: 24, background: open ? C.magenta : C.crack }} />
                      <div className="head" style={{ flex: 1, fontSize: 19 }}>{g.label}</div>
                      <span style={{ fontSize: 11, color: C.dust }}>{g.rows.length} kw</span>
                    </div>
                    {open && (
                      <div style={{ borderTop: '1px solid ' + C.crack, overflowX: 'auto', maxHeight: 400, overflowY: 'auto' }}>
                        <table style={{ width: '100%', borderCollapse: 'collapse' }}>
                          <thead><tr style={{ background: C.slab }}>
                            {['Keyword', 'URL', 'Volume', 'Source'].map(h => <th key={h} style={{ padding: '10px 16px', textAlign: 'left', fontWeight: 700, fontSize: 10, color: C.dust }}>{h}</th>)}
                          </tr></thead>
                          <tbody>
                            {g.rows.slice().sort((a, b) => b.sv - a.sv).map((r, i) => (
                              <tr key={i} style={{ borderTop: '1px solid ' + C.crack }}>
                                <td style={{ padding: '9px 16px', color: C.white, fontWeight: 500, fontSize: 12 }}>{r.keyword}</td>
                                <td style={{ padding: '9px 16px' }}>{r.url === 'N/A' ? <span style={{ fontFamily: 'monospace', fontSize: 11, color: C.indigo }}>N/A</span> : <span style={{ fontFamily: 'monospace', fontSize: 10, color: C.dust }}>{r.url.substring(0, 50)}</span>}</td>
                                <td style={{ padding: '9px 16px', color: C.volt, fontWeight: 700, fontSize: 12 }}>{r.sv.toLocaleString()}</td>
                                <td style={{ padding: '9px 16px', fontSize: 10 }}><span style={{ background: r.source === 'Client' ? C.magenta + '1a' : C.indigo + '1a', color: r.source === 'Client' ? C.magenta : C.indigo, padding: '2px 9px', fontWeight: 700 }}>{r.source}</span></td>
                              </tr>
                            ))}
                          </tbody>
                        </table>
                      </div>
                    )}
                  </div>
                );
              })}
            </div>
          </div>
        )}

      </div>
    </div>
  );
}
