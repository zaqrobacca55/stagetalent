[index.html](https://github.com/user-attachments/files/28694310/index.html)
<!DOCTYPE html>
<html lang="ja">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ステージ プロフィール変換ツール</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,500;0,600;1,500;1,600&family=Noto+Serif+JP:wght@500;600&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
<style>
  :root{
    --bg:#f6f3ee;          /* やわらかいアイボリー（サイトの白基調） */
    --bg2:#efe9e0;
    --panel:#ffffff;
    --panel2:#faf7f2;
    --line:#e3dccf;
    --line-strong:#d6cdba;
    --ink:#2b2620;         /* 黒に近いダークブラウン */
    --sub:#8a8070;
    --accent:#b89248;      /* ステージのゴールド */
    --accent-deep:#9a7833;
    --accent2:#a8442f;     /* 差し色の赤茶 */
    --ok:#5b7a52;
    --field:#fbfaf7;
  }
  *{box-sizing:border-box;}
  body{
    margin:0;
    font-family:"Hiragino Mincho ProN","Yu Mincho",YuMincho,"Noto Serif JP",serif;
    background:
      radial-gradient(1100px 600px at 88% -8%, rgba(184,146,72,.10), transparent 60%),
      radial-gradient(800px 600px at -8% 108%, rgba(168,68,47,.05), transparent 55%),
      var(--bg);
    color:var(--ink);
    line-height:1.75;
    min-height:100vh;
  }
  .serif-en{font-family:"Cormorant Garamond",Georgia,"Times New Roman",serif;}

  /* ===== ヘッダー：サイト風 英字大見出し＋日本語サブ ===== */
  header{
    position:relative;
    padding:34px 32px 28px;
    border-bottom:1px solid var(--line);
    background:linear-gradient(180deg,#ffffff, #fbf8f3);
    overflow:hidden;
  }
  header::after{
    content:"";position:absolute;left:0;right:0;bottom:0;height:3px;
    background:linear-gradient(90deg,var(--accent),var(--accent-deep) 45%,transparent);
  }
  header .head-inner{
    max-width:1180px;margin:0 auto;display:flex;align-items:center;gap:22px;flex-wrap:wrap;
  }
  header .logo{
    font-family:"Cormorant Garamond",Georgia,serif;
    font-style:italic;font-weight:600;
    font-size:30px;letter-spacing:.18em;
    color:var(--ink);
    line-height:1;
    padding-right:22px;border-right:1px solid var(--line-strong);
  }
  header .logo small{
    display:block;font-style:normal;font-family:inherit;
    font-size:9.5px;letter-spacing:.42em;color:var(--accent-deep);
    margin-top:6px;font-weight:600;
  }
  header h1{
    font-size:20px;margin:0;font-weight:600;letter-spacing:.06em;color:var(--ink);
  }
  header p{margin:4px 0 0;color:var(--sub);font-size:13px;font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;}

  .wrap{max-width:1180px;margin:0 auto;padding:34px 32px 90px;}
  .grid{display:grid;grid-template-columns:minmax(0,1fr) minmax(0,1.05fr);gap:30px;align-items:start;}
  @media(max-width:920px){.grid{grid-template-columns:1fr;}}

  /* ===== ドロップエリア ===== */
  .drop{
    display:block;width:100%;
    border:1.5px dashed var(--line-strong);
    border-radius:4px;
    padding:38px 22px;
    text-align:center;cursor:pointer;transition:.2s;
    background:var(--panel);
  }
  .drop:hover,.drop.hot{border-color:var(--accent);background:var(--panel2);box-shadow:0 6px 24px rgba(184,146,72,.10);}
  .drop .big{font-size:15px;margin-bottom:6px;font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;}
  .drop .sub{color:var(--sub);font-size:12.5px;font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;}
  .drop .ico{font-size:30px;margin-bottom:10px;color:var(--accent);}
  #file{display:none;}

  /* ===== カード ===== */
  .card{
    background:var(--panel);border:1px solid var(--line);border-radius:4px;
    padding:24px 22px 24px;box-shadow:0 1px 0 #fff inset, 0 6px 22px rgba(43,38,32,.04);
  }
  .card h2{
    position:relative;
    font-family:"Cormorant Garamond",Georgia,serif;
    font-size:22px;font-style:italic;letter-spacing:.04em;color:var(--accent-deep);
    margin:0 0 4px;font-weight:600;
  }
  .card h2 .ja{
    display:block;font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;
    font-style:normal;font-size:12px;letter-spacing:.14em;color:var(--sub);
    margin-top:2px;font-weight:600;
  }
  .card h2 + *{margin-top:18px;}

  .fld{margin-bottom:14px;}
  .fld label{display:block;font-size:11.5px;color:var(--sub);margin-bottom:5px;letter-spacing:.04em;
    font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;}
  .fld input,.fld textarea,.fld select{
    width:100%;background:var(--field);border:1px solid var(--line-strong);
    color:var(--ink);border-radius:3px;padding:10px 12px;font-size:14px;
    font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;transition:.15s;
  }
  .fld input:focus,.fld textarea:focus,.fld select:focus{
    outline:none;border-color:var(--accent);box-shadow:0 0 0 3px rgba(184,146,72,.15);background:#fff;
  }
  .fld textarea{resize:vertical;min-height:62px;}
  .row3{display:grid;grid-template-columns:repeat(3,1fr);gap:10px;}
  .row2{display:grid;grid-template-columns:repeat(2,1fr);gap:10px;}

  .btns{display:flex;gap:12px;margin-top:6px;flex-wrap:wrap;}
  button{
    font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;
    cursor:pointer;border:none;border-radius:3px;
    padding:12px 24px;font-size:14px;font-weight:600;letter-spacing:.05em;transition:.18s;
  }
  .b-main{background:linear-gradient(135deg,var(--accent),var(--accent-deep));color:#fff;box-shadow:0 4px 14px rgba(184,146,72,.28);}
  .b-main:hover{filter:brightness(1.05);transform:translateY(-1px);}
  .b-ghost{background:transparent;color:var(--ink);border:1px solid var(--line-strong);}
  .b-ghost:hover{border-color:var(--accent);color:var(--accent-deep);}
  .b-copy{background:var(--ok);color:#fff;}
  .b-copy:hover{filter:brightness(1.06);}

  .out{margin-top:30px;}
  .out textarea{
    width:100%;min-height:320px;background:#2b2620;border:1px solid #1f1b16;
    color:#e8dcc4;border-radius:4px;padding:16px;font-size:12.5px;
    font-family:"SF Mono",Consolas,Menlo,monospace;line-height:1.6;
  }
  .note{font-size:12px;color:var(--sub);margin-top:9px;font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;}
  .raw details{margin-top:18px;}
  .raw summary{cursor:pointer;color:var(--sub);font-size:12px;font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;}
  .raw pre{
    background:#faf7f2;border:1px solid var(--line);border-radius:4px;
    padding:12px;font-size:11.5px;color:#6b6354;white-space:pre-wrap;
    max-height:240px;overflow:auto;margin-top:8px;
  }
  .toast{
    position:fixed;bottom:26px;left:50%;transform:translateX(-50%) translateY(20px);
    background:var(--ink);color:#f6f3ee;padding:12px 28px;border-radius:30px;
    font-weight:600;font-size:14px;opacity:0;pointer-events:none;transition:.25s;z-index:50;
    font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;
  }
  .toast.show{opacity:1;transform:translateX(-50%) translateY(0);}
  .imgnote{font-size:11.5px;color:var(--accent-deep);background:rgba(184,146,72,.09);
    border:1px solid rgba(184,146,72,.28);border-radius:4px;padding:10px 12px;margin-bottom:14px;
    font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;}
  .status{font-size:12.5px;color:var(--sub);margin-top:10px;min-height:18px;
    font-family:"Hiragino Kaku Gothic ProN","Yu Gothic",Meiryo,sans-serif;}
  .status.ok{color:var(--ok);}
  .status.err{color:var(--accent2);}
</style>
</head>
<body>
<header>
  <div class="head-inner">
    <span class="logo">STAGE<small>MODEL &amp; TALENT</small></span>
    <div>
      <h1>プロフィール PDF → HTML 変換ツール</h1>
      <p>PDFをアップロード → 自動抽出 → 内容を確認・修正 → HTMLを生成</p>
    </div>
  </div>
</header>

<div class="wrap">
  <div class="grid">
    <!-- LEFT -->
    <div>
      <label class="drop" id="drop" for="file">
        <div class="ico"><svg width="40" height="40" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.3" stroke-linecap="round" stroke-linejoin="round"><path d="M14 3v5h5"/><path d="M19 21H7a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h7l5 5v11a2 2 0 0 1-2 2Z"/><path d="M9 13h6M9 17h4"/></svg></div>
        <div class="big">プロフィールPDFをドロップ / クリックで選択</div>
        <div class="sub">ステージ形式のプロフィールPDFに対応</div>
      </label>
      <input type="file" id="file" accept="application/pdf">
      <div class="status" id="status"></div>

      <div class="card" style="margin-top:22px;">
        <h2>Profile<span class="ja">抽出結果（確認・修正）</span></h2>

        <div class="imgnote">
          ✓ 画像URLは「年月＋ローマ字姓」から自動生成されます。年月は今日の日付（ツール使用日）が初期値です。必要なら下で調整できます。
        </div>

        <div class="row2">
          <div class="fld"><label>氏名（漢字）</label><input id="f_name" placeholder="例：山田　花子"></div>
          <div class="fld"><label>氏名（ローマ字・自動）</label><input id="f_roma" placeholder="例：Yamada Hanako"></div>
        </div>
        <div class="row2">
          <div class="fld"><label>かな姓（区切り確認用）</label><input id="f_kana_sei" placeholder="例：やまだ"></div>
          <div class="fld"><label>かな名</label><input id="f_kana_mei" placeholder="例：はなこ"></div>
        </div>
        <div class="note" style="margin:-4px 0 12px;">※かなの姓名の区切りは自動推定です。違っていたら直すと、ローマ字氏名と画像slugに反映されます。</div>
        <div class="fld"><label>生年月日</label><input id="f_birth" placeholder="例：1980年11月13日"></div>
        <div class="row3">
          <div class="fld"><label>B</label><input id="f_b" placeholder="例：80cm"></div>
          <div class="fld"><label>W</label><input id="f_w" placeholder="例：65cm"></div>
          <div class="fld"><label>H</label><input id="f_h" placeholder="例：89cm"></div>
        </div>
        <div class="row2">
          <div class="fld"><label>身長</label><input id="f_height" placeholder="例：158cm"></div>
          <div class="fld"><label>靴のサイズ</label><input id="f_shoe" placeholder="例：23cm"></div>
        </div>
        <div class="fld"><label>出身地</label><input id="f_from" placeholder="例：東京都"></div>
        <div class="fld"><label>趣味（・区切り。改行は・に変換されます）</label><textarea id="f_hobby" placeholder="趣味を記入"></textarea></div>
        <div class="fld"><label>特技（・区切り）</label><input id="f_skill" placeholder="特技を記入"></div>
        <div class="fld"><label>資格（・区切り。無ければ空欄でOK）</label><input id="f_license" placeholder="資格を記入"></div>
        <div class="fld">
          <label>出演情報（1行＝1項目。「区分: 内容」の形式。区分は CM / WEB / TV など）</label>
          <textarea id="f_credits" style="min-height:120px;" placeholder="出演情報を記入"></textarea>
        </div>

        <h2 style="margin-top:6px;">Image<span class="ja">画像URL（自動生成）</span></h2>
        <div class="row3">
          <div class="fld"><label>年</label><input id="i_year" placeholder="2026"></div>
          <div class="fld"><label>月（2桁）</label><input id="i_month" placeholder="06"></div>
          <div class="fld"><label>ローマ字姓（小文字・画像用）</label><input id="i_slug" placeholder="例：yamada"></div>
        </div>
        <div class="fld"><label>生成されるベースURL（自動・編集可）</label><input id="i_preview" placeholder="例：https://stage-model.com/wp-content/uploads/2026/06/yamada"></div>
        <div class="fld"><label>性別（alt自動振り分け用）</label>
          <select id="i_gender">
            <option value="female">女性</option>
            <option value="male">男性</option>
          </select>
        </div>
        <div class="fld"><label>alt属性テキスト（性別・年齢から自動／編集可）</label><input id="i_alt" placeholder="例：40代・50代の女性ミドルモデル・タレント　山田　花子"></div>

        <div class="btns" style="margin-top:8px;">
          <button class="b-main" id="gen">HTMLを生成 ↓</button>
          <button class="b-ghost" id="clear">クリア</button>
        </div>

        <div class="raw">
          <details>
            <summary>PDFから読み取った生テキストを見る</summary>
            <pre id="rawtext">（まだPDFを読み込んでいません）</pre>
          </details>
        </div>
      </div>
    </div>

    <!-- RIGHT -->
    <div>
      <div class="card">
        <h2>Output<span class="ja">生成されたHTML</span></h2>
        <div class="btns" style="margin-bottom:14px;">
          <button class="b-copy" id="copy">📋 コピー</button>
          <button class="b-ghost" id="download">.html で保存</button>
        </div>
        <div class="out"><textarea id="output" spellcheck="false" placeholder="左で内容を確認して「HTMLを生成」を押すと、ここにコードが出ます。"></textarea></div>
        <div class="note">生成されたコードは、ステージのプロフィールテンプレートと同じ構造です。そのままCMSの本文に貼り付けられます。</div>
      </div>
    </div>
  </div>
</div>

<div class="toast" id="toast">コピーしました</div>

<script>
pdfjsLib.GlobalWorkerOptions.workerSrc = "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.worker.min.js";

const $ = id => document.getElementById(id);
const drop = $("drop"), fileInput = $("file"), statusEl = $("status");

// ---------- alt自動振り分け ----------
// 生年月日文字列から満年齢を計算（1980年11月13日 / 1980.11.13 などに対応）
function calcAge(birthStr){
  if(!birthStr) return null;
  const m = birthStr.match(/(\d{4})\D+(\d{1,2})\D+(\d{1,2})/);
  if(!m) return null;
  const by=+m[1], bm=+m[2], bd=+m[3];
  const now=new Date();
  let age=now.getFullYear()-by;
  const md=(now.getMonth()+1-bm)||(now.getDate()-bd);
  if(md<0) age--;
  return age;
}
// 年齢→年代区分（〜30代 / 40代・50代 / 60代・70代・80代）と肩書き（ミドル/シニア）
function ageBand(age){
  if(age==null) return null;
  if(age<40) return {band:"〜30代", title:"ミドル"};
  if(age<60) return {band:"40代・50代", title:"ミドル"};
  return {band:"60代・70代・80代", title:"シニア"};
}
// 名前（かな名・漢字名）から性別を推定。確信度が低ければ null を返す。
// 戻り値: "male" | "female" | null
function guessGenderByName(kanaMei, kanjiName){
  const km = (kanaMei||"").trim();
  const kj = (kanjiName||"").replace(/[\s　]/g,"").trim();
  // ---- 女性寄りの手がかり ----
  // 漢字名の末尾（子/美/香/恵/奈/菜/絵/江/代/乃/里/紀/穂/花/華/愛/桜）
  const femaleKanjiTail = /(子|美|香|恵|奈|菜|絵|江|代|乃|里|紀|穂|花|華|愛|桜|彩|綾|萌|麻)$/;
  // ---- 男性寄りの手がかり ----
  // かな名末尾（男性に多い終わり）。「ま」は女性名でも使われるため除外。
  const maleKanaTail = /(ろう|すけ|へい|ぞう|いち|ぺい|だい|や|じ|ご)$/;
  // 女性に多いかな末尾
  const femaleKanaTail = /(こ|み|な|か|え|よ|り|ん|ほ|ゆ|さ|ま)$/;
  // 漢字名末尾（郎/男/夫/雄/彦/介/助/也/哉/太/斗/翔/輝/樹/健/剛/司/治/平/吾/蔵/造）
  const maleKanjiTail = /(郎|男|夫|雄|彦|介|助|也|哉|太|斗|翔|輝|樹|健|剛|司|治|平|吾|蔵|造|之|信|義|和|秀)$/;

  let score = 0; // 正=女性, 負=男性
  if(femaleKanjiTail.test(kj)) score += 2;
  if(maleKanjiTail.test(kj))   score -= 2;
  if(maleKanaTail.test(km))    score -= 2;
  if(femaleKanaTail.test(km) && !maleKanaTail.test(km)) score += 1;

  if(score >= 1) return "female";
  if(score <= -1) return "male";
  return null; // 判定できない
}

// 性別＋年齢＋氏名からalt文字列を組み立て
function buildAlt(){
  const gender = $("i_gender").value==="male" ? "男性" : "女性";
  const age = calcAge($("f_birth").value.trim());
  const ab = ageBand(age);
  const name = $("f_name").value.trim();
  if(!ab){
    // 年齢が判定できないときは年代部分を空にせず、デフォルトをミドル40代・50代に
    return `40代・50代の${gender}ミドルモデル・タレント　${name}`.trimEnd();
  }
  return `${ab.band}の${gender}${ab.title}モデル・タレント　${name}`.trimEnd();
}
// 入力変化でalt自動更新（手で編集していたら上書きしない）
let altEdited=false;
function refreshAlt(){
  if(altEdited) return;
  $("i_alt").value = buildAlt();
}

// ---------- ドラッグ&ドロップ ----------
["dragenter","dragover"].forEach(e=>drop.addEventListener(e,ev=>{ev.preventDefault();drop.classList.add("hot");}));
["dragleave","drop"].forEach(e=>drop.addEventListener(e,ev=>{ev.preventDefault();drop.classList.remove("hot");}));
drop.addEventListener("drop",ev=>{const f=ev.dataTransfer.files[0]; if(f) handleFile(f);});
fileInput.addEventListener("change",ev=>{const f=ev.target.files[0]; if(f) handleFile(f);});

function setStatus(msg,cls){statusEl.textContent=msg;statusEl.className="status"+(cls?" "+cls:"");}

// ---------- 画像URL自動生成 ----------
const IMG_BASE = "https://stage-model.com/wp-content/uploads/";
function initDate(){
  const d=new Date();
  $("i_year").value=String(d.getFullYear());
  $("i_month").value=String(d.getMonth()+1).padStart(2,"0");
}
function romaToSlug(roma){
  // 「Hanako Yamada」→ 姓(最後の語) yamada。1語なら全部を採用。
  const parts=(roma||"").trim().split(/\s+/).filter(Boolean);
  if(!parts.length) return "";
  const last=parts[parts.length-1];
  return last.toLowerCase().replace(/[^a-z]/g,"");
}
function updatePreview(){
  const y=$("i_year").value.trim();
  const m=$("i_month").value.trim();
  const slug=$("i_slug").value.trim();
  if(y&&m&&slug){
    $("i_preview").value=`${IMG_BASE}${y}/${m}/${slug}`;
  }
}
["i_year","i_month","i_slug"].forEach(id=>$(id).addEventListener("input",updatePreview));
// alt自動振り分け：性別・生年月日・氏名の変化で再生成。altを手で触ったら自動停止。
["i_gender","f_birth","f_name"].forEach(id=>$(id).addEventListener("input",refreshAlt));
$("i_gender").addEventListener("change",refreshAlt);
$("i_alt").addEventListener("input",()=>{altEdited=true;});
initDate();

// ---------- PDF読み込み ----------
async function handleFile(file){
  if(file.type!=="application/pdf"){setStatus("PDFファイルを選んでください。","err");return;}
  setStatus("読み込み中…");
  try{
    const buf = await file.arrayBuffer();
    const pdf = await pdfjsLib.getDocument({data:buf}).promise;
    let text="";
    for(let p=1;p<=pdf.numPages;p++){
      const page=await pdf.getPage(p);
      const c=await page.getTextContent();
      // Y座標でおおまかに行を組む
      const items=c.items.map(it=>({s:it.str,x:it.transform[4],y:it.transform[5]}));
      items.sort((a,b)=> b.y-a.y || a.x-b.x);
      let lastY=null,line="";
      items.forEach(it=>{
        if(lastY===null){line=it.s;lastY=it.y;return;}
        if(Math.abs(it.y-lastY)>3){text+=line+"\n";line=it.s;}
        else{line+=it.s;}
        lastY=it.y;
      });
      text+=line+"\n";
    }
    $("rawtext").textContent=text;
    parseFields(text);
    setStatus("抽出しました。内容を確認・修正してください。","ok");
  }catch(err){
    console.error(err);
    setStatus("読み込みに失敗しました: "+err.message,"err");
  }
}

// ---------- かな→ローマ字（ヘボン式・簡易） ----------
const KANA_MAP={
  "きゃ":"kya","きゅ":"kyu","きょ":"kyo","しゃ":"sha","しゅ":"shu","しょ":"sho",
  "ちゃ":"cha","ちゅ":"chu","ちょ":"cho","にゃ":"nya","にゅ":"nyu","にょ":"nyo",
  "ひゃ":"hya","ひゅ":"hyu","ひょ":"hyo","みゃ":"mya","みゅ":"myu","みょ":"myo",
  "りゃ":"rya","りゅ":"ryu","りょ":"ryo","ぎゃ":"gya","ぎゅ":"gyu","ぎょ":"gyo",
  "じゃ":"ja","じゅ":"ju","じょ":"jo","びゃ":"bya","びゅ":"byu","びょ":"byo",
  "ぴゃ":"pya","ぴゅ":"pyu","ぴょ":"pyo",
  "あ":"a","い":"i","う":"u","え":"e","お":"o",
  "か":"ka","き":"ki","く":"ku","け":"ke","こ":"ko",
  "さ":"sa","し":"shi","す":"su","せ":"se","そ":"so",
  "た":"ta","ち":"chi","つ":"tsu","て":"te","と":"to",
  "な":"na","に":"ni","ぬ":"nu","ね":"ne","の":"no",
  "は":"ha","ひ":"hi","ふ":"fu","へ":"he","ほ":"ho",
  "ま":"ma","み":"mi","む":"mu","め":"me","も":"mo",
  "や":"ya","ゆ":"yu","よ":"yo",
  "ら":"ra","り":"ri","る":"ru","れ":"re","ろ":"ro",
  "わ":"wa","を":"o","ん":"n",
  "が":"ga","ぎ":"gi","ぐ":"gu","げ":"ge","ご":"go",
  "ざ":"za","じ":"ji","ず":"zu","ぜ":"ze","ぞ":"zo",
  "だ":"da","ぢ":"ji","づ":"zu","で":"de","ど":"do",
  "ば":"ba","び":"bi","ぶ":"bu","べ":"be","ぼ":"bo",
  "ぱ":"pa","ぴ":"pi","ぷ":"pu","ぺ":"pe","ぽ":"po",
  "ー":""
};
function kanaToRoma(kana){
  let s=kana.replace(/\s+/g,""),out="",i=0;
  while(i<s.length){
    // 促音
    if(s[i]==="っ"){ const nx=s.substr(i+1,2),nx1=s[i+1];
      const r=KANA_MAP[nx]||KANA_MAP[nx1]; if(r) out+=r[0]; i++; continue; }
    const two=s.substr(i,2);
    if(KANA_MAP[two]!==undefined){out+=KANA_MAP[two];i+=2;continue;}
    const one=s[i];
    if(KANA_MAP[one]!==undefined){out+=KANA_MAP[one];i+=1;continue;}
    out+=one;i+=1;
  }
  return out;
}
function cap(s){return s?s[0].toUpperCase()+s.slice(1):"";}

// ---------- フィールド抽出 ----------
function parseFields(t){
  // 制御文字（\x08 等）と単位前のゴミを除去
  t=t.replace(/[\x00-\x07\x08\x0b\x0c\x0e-\x1f]/g,"");
  const lines=t.split("\n").map(s=>s.trim()).filter(Boolean);
  const joined=t.replace(/\s+/g," ");
  const grab=(re,grp=1)=>{const m=joined.match(re);return m?m[grp].trim():"";};

  // 生年月日: 1980.11.13 → 1980年11月13日
  const bd=grab(/(\d{4})[.\/](\d{1,2})[.\/](\d{1,2})/,0);
  if(bd){
    const m=bd.match(/(\d{4})[.\/](\d{1,2})[.\/](\d{1,2})/);
    $("f_birth").value=`${m[1]}年${+m[2]}月${+m[3]}日`;
  }

  // 性別：PDF内の「性別 男/女」等を最優先で採用。明示が無ければ後段で名前から推定。
  let genderFromPDF=false;
  {
    const gm = joined.match(/性\s*別\s*[：:]?\s*(男|女)/);
    if(gm){ $("i_gender").value = gm[1]==="男" ? "male" : "female"; genderFromPDF=true; }
  }

  // 氏名: かな氏名行（ひらがなのみ）と漢字氏名行を探す
  let kanji="",kana="",kanaHasSpace=false,kanaSeiGuess="";
  for(let i=0;i<lines.length;i++){
    if(/^[ぁ-ん\u30fc\s]+$/.test(lines[i]) && lines[i].replace(/\s/g,"").length>=3 && lines[i].length<=14){
      // かな行に元々スペースがあれば、それを姓名の区切りとして優先採用
      const parts=lines[i].trim().split(/[\s　]+/).filter(Boolean);
      if(parts.length>=2){ kanaHasSpace=true; kanaSeiGuess=parts[0]; }
      kana=lines[i].replace(/\s/g,"");
      for(const j of [i-1,i+1]){
        if(lines[j] && /[一-龯]/.test(lines[j]) && lines[j].replace(/\s/g,"").length<=8 &&
           !/cm|kg|歳|年|月|日|区|都|道|府|県|株式|趣味|特技|資格/.test(lines[j])){
          kanji=lines[j].replace(/\s/g,"");break;
        }
      }
      if(kanji)break;
    }
  }

  // かな姓名の分割数を先に決める（漢字側の分割位置推定にも使う）
  let kanaSeiLen=0;
  if(kana){
    if(kanaHasSpace && kanaSeiGuess && kanaSeiGuess.length<kana.length){
      kanaSeiLen=kanaSeiGuess.length;
    }else{
      kanaSeiLen=Math.ceil(kana.length/2);
    }
  }

  if(kanji){
    // 姓名を全角スペースで区切る。かな姓の文字数比から漢字の区切り位置を推定し、
    // 推定できないときは中央で割る。1文字はそのまま。
    let disp=kanji;
    if(kanji.length>=2){
      let cut;
      if(kana && kanaSeiLen>0 && kanaSeiLen<kana.length){
        // かな全体に対する姓の割合を漢字長に按分（最低1・最大 長さ-1）
        cut=Math.round(kanji.length*(kanaSeiLen/kana.length));
        cut=Math.min(Math.max(cut,1),kanji.length-1);
      }else{
        cut=Math.ceil(kanji.length/2);
      }
      disp=kanji.slice(0,cut)+"　"+kanji.slice(cut);
    }
    $("f_name").value=disp;
    altEdited=false; refreshAlt();
  }else if(kana){
    // 漢字氏名が無い（ひらがな表記のタレント）場合は、かな氏名を氏名欄に採用。
    // 姓名の区切り位置（kanaSeiLen）で全角スペースを入れる。
    let disp=kana;
    if(kanaSeiLen>0 && kanaSeiLen<kana.length){
      disp=kana.slice(0,kanaSeiLen)+"　"+kana.slice(kanaSeiLen);
    }
    $("f_name").value=disp;
    altEdited=false; refreshAlt();
  }
  // かな姓名の推定分割
  if(kana){
    $("f_kana_sei").value=kana.slice(0,kanaSeiLen);
    $("f_kana_mei").value=kana.slice(kanaSeiLen);
    syncRoma();
  }

  // 性別：PDFに明示が無ければ、名前のヒントから初期値を推定（外れたら手で修正）
  if(!genderFromPDF){
    const g = guessGenderByName(kana ? kana.slice(kanaSeiLen) : "", kanji);
    if(g){ $("i_gender").value = g; }
  }
  // 名前・性別が揃った状態でaltを再生成
  altEdited=false; refreshAlt();

  // サイズ：T B W H S は常にこの並び順で出る。ラベルの出現順と数値cmの出現順を
  // それぞれ取り出し、位置で対応づける（値→ラベル/ラベル→値の両レイアウトに頑健）。
  const sizeMap={};
  {
    // ラベル出現順（T B W H S）
    const labelSeq=[];
    const labRe=/[ＴTＢBＷWＨHＳS]\s*[：:]/g; let lm;
    while((lm=labRe.exec(joined))!==null){
      const ch=lm[0][0];
      const norm={"Ｔ":"T","T":"T","Ｂ":"B","B":"B","Ｗ":"W","W":"W","Ｈ":"H","H":"H","Ｓ":"S","S":"S"}[ch];
      if(norm && !labelSeq.includes(norm)) labelSeq.push(norm);
    }
    // 数値cm 出現順（身長の156cmやKgは除くため、後で対応）
    const nums=[]; const numRe=/([0-9]+(?:\.[0-9]+)?)\s*c\s*m/gi; let nm;
    while((nm=numRe.exec(joined))!==null) nums.push(nm[1]);
    // ラベルの直前/直後どちらに値が来るか判定：T(身長)を含む全体で対応づけ
    // 典型: T,B,W,H,S(5ラベル) と 数値cm が同数前後で並ぶ。順番で割り当てる。
    if(labelSeq.length && nums.length){
      // 身長(T)は別欄。サイズラベルB/W/H/Sに対し、cm値を順に割当。
      // 値の個数がラベルと一致する場合は素直に対応。多い場合は末尾の身長cmを除く。
      let vals=nums.slice();
      // labelSeqにTが含まれる場合、身長cm(=156等、通常Kgが直後)を特定して割当
      const heightMatch=joined.match(/([0-9]+(?:\.[0-9]+)?)\s*c\s*m\s*[0-9]+(?:\.[0-9]+)?\s*Kg/i);
      const heightVal=heightMatch?heightMatch[1]:null;
      labelSeq.forEach((lab,idx)=>{
        if(lab==="T"){
          if(heightVal) sizeMap.T=heightVal;
          return;
        }
        // T以外: 対応する値を順に取る（身長値はスキップ）
        while(vals.length && heightVal && vals[0]===heightVal) vals.shift();
        if(vals.length) sizeMap[lab]=vals.shift();
      });
    }
  }
  $("f_b").value=sizeMap.B?sizeMap.B+"cm":"";
  $("f_w").value=sizeMap.W?sizeMap.W+"cm":"";
  $("f_h").value=sizeMap.H?sizeMap.H+"cm":"";
  $("f_shoe").value=sizeMap.S?sizeMap.S+"cm":"";
  // 身長
  let ht=sizeMap.T || grab(/([0-9]+(?:\.[0-9]+)?)\s*c\s*m\s*[0-9]+(?:\.[0-9]+)?\s*Kg/i);
  if(ht) $("f_height").value=ht.replace(/\.0+$/,"")+"cm";

  // 出身地（ラベル直後に値が無く、別行に都道府県が出るPDFにも対応）
  let from=grab(/出身地[：:\s]*([^\s：:]+(?:都|道|府|県))/);
  if(!from){
    const pref=lines.find(l=>/^(北海道|[^\s]{1,4}[都府県])$/.test(l.replace(/\s/g,"")));
    if(pref) from=pref.replace(/\s/g,"");
  }
  $("f_from").value=from;

  // 趣味・特技・資格（全角/半角スペース区切り→・に統一）
  $("f_hobby").value=normSep(pickBlock(lines,/趣味/,/特技|資格|芸歴|出身|出演/));
  $("f_skill").value=normSep(pickBlock(lines,/特技/,/資格|芸歴|趣味|出演|出身|WEB|TV|CM/));
  $("f_license").value=normSep(pickBlock(lines,/資格/,/芸歴|趣味|特技|出演|出身|WEB|TV|CM|株式/));

  // 出演情報：【区分】の後ろのテキストを内容として取得（行の並びに依存しない）
  // joined全文から「【…】内容【…】内容…」を順に切り出す。
  const credits=[];
  const markerRe=/[【\[]\s*([^】\]]{1,12}?)\s*[】\]]/g;
  let mk, markers=[];
  while((mk=markerRe.exec(joined))!==null){
    markers.push({label:mk[1].trim(), start:mk.index, end:markerRe.lastIndex});
  }
  for(let k=0;k<markers.length;k++){
    const lab=markers[k].label.toUpperCase().replace(/\s/g,"");
    if(!/^(CM|WEB|TV|MOVIE|映画|舞台|ラジオ|RADIO|雑誌|広告|MV|PV|その他|ドラマ|配信|広報|モデル|出演|ナレーション)$/i.test(lab)) continue;
    // 内容 = このマーカー末尾 〜 次マーカー先頭（または芸歴セクション終端）
    const sliceEnd = (k+1<markers.length) ? markers[k+1].start : joined.length;
    let body=joined.slice(markers[k].end, sliceEnd);
    // 終端ワードで打ち切り
    body=body.split(/生年月日|年齢|性別|出身地|趣味|特技|資格|身長|株式会社|E-mail|https|TEL|FAX|芸歴|Page:|[ＴTＢBＷWＨHＳS]\s*[：:]|\d{4}[.\/]\d{1,2}[.\/]\d{1,2}/)[0];
    body=body.replace(/\s+/g," ").trim();
    if(body) credits.push(`${lab}: ${body}`);
  }
  if(credits.length) $("f_credits").value=credits.join("\n");

  updatePreview();
}

// かな姓→ローマ字でslug、フルネームでローマ字氏名
function syncRoma(){
  const sei=$("f_kana_sei").value.trim();
  const mei=$("f_kana_mei").value.trim();
  const romaSei=cap(kanaToRoma(sei));
  const romaMei=cap(kanaToRoma(mei));
  if(romaSei||romaMei) $("f_roma").value=(romaSei+" "+romaMei).trim();
  if(romaSei){ $("i_slug").value=romaSei.toLowerCase(); }
  updatePreview();
}
["f_kana_sei","f_kana_mei"].forEach(id=>document.getElementById(id).addEventListener("input",syncRoma));

function normSep(s){return s.replace(/[\n\u3000\s]+/g,"・").replace(/^・|・$/g,"").trim();}

function fmtCm(v){return v?v.replace(/cm$/i,"")+"cm":"";}

// ラベル行の次行から終端ラベルまでを集める
function pickBlock(lines,startRe,endRe){
  let out=[],on=false;
  for(const ln of lines){
    if(startRe.test(ln)){
      on=true;
      const rest=ln.replace(/.*?(趣味|特技|資格)[：:\s]*/,"").trim();
      if(rest && !startRe.test(rest)) out.push(rest);
      continue;
    }
    if(on){
      if(endRe.test(ln)) break;
      if(/cm|kg|歳|生年|性別|名前|株式|E-mail|https/.test(ln)) break;
      out.push(ln);
    }
  }
  return out.join("\n").trim();
}

function pickAfterLabel(lines,startRe,endRe){
  let out=[],on=false;
  for(const ln of lines){
    if(startRe.test(ln)){on=true;continue;}
    if(on){
      if(endRe.test(ln)) break;
      out.push(ln);
    }
  }
  return out.join("\n").trim();
}

// ---------- HTML生成 ----------
function esc(s){return (s||"").replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;");}
function nl2br(s){return esc(s).replace(/\n/g,"<br>\n\t\t\t\t\t\t\t");}

function buildHTML(){
  const v=id=>$(id).value.trim();
  const alt=esc(v("i_alt"));
  const base=v("i_preview")||(()=>{updatePreview();return v("i_preview");})();
  const img=(n)=>base?`\t\t\t\t\t<li><img src="${esc(base)}${n}.jpg" alt="${alt}" /></li>`:"";
  const mainImgs=[img("01"),img("02")].filter(Boolean).join("\n");
  const thumbImgs=[img("03"),img("04")].filter(Boolean).join("\n");

  // 値が空の項目はdlごと省略する
  const dlOpt=(label,val,raw)=> val ? `
					<dl>
						<dt>${label}</dt>
						<dd>${raw?val:nl2br(val)}</dd>
					</dl>` : "";
  const fromDl    = dlOpt("出身地", esc(v("f_from")), true);
  const hobbyDl   = dlOpt("趣味",   v("f_hobby"));
  const skillDl   = dlOpt("特技",   v("f_skill"));
  const licenseDl = dlOpt("資格",   v("f_license"));

  // 出演情報を「区分: 内容」の各行から動的に生成
  const creditLines = v("f_credits").split("\n").map(s=>s.trim()).filter(Boolean);
  const creditDls = creditLines.map((line,ci)=>{
    const idx=line.indexOf(":");
    const idx2=line.indexOf("：");
    const cut = idx>=0 && (idx2<0||idx<idx2) ? idx : (idx2>=0?idx2:-1);
    let label,body;
    if(cut>=0){ label=line.slice(0,cut).trim(); body=line.slice(cut+1).trim(); }
    else { label=line.trim(); body=""; }
    // 最後の出演情報の末尾には「他」を付与（<br><br>他）
    const ddInner = (ci===creditLines.length-1)
      ? `${nl2br(body)}<br><br>\n\t\t\t\t\t\t\t\t\t他`
      : nl2br(body);
    return `\t\t\t\t\t\t\t\t<dl>
									<dt>${esc(label)}</dt>
									<dd>${ddInner}</dd>
								</dl>`;
  }).join("\n");

  return `<div class="profile_first">
	<div class="inner">
		<div class="topline">
			<div class="hero_img">
				<div class="btn_color bg_db2 center long">
					<a href="https://stage-model.com/casting/">
						<p class="noto"><span>お仕事の依頼はこちら</span></p>
					</a>
				</div>
				<ul class="slider slick-mainview">
${mainImgs}
				</ul>
				<ul class="slider slick-thumb">
${thumbImgs}
				</ul>&nbsp;
				<div class="btn_color bg_db2 center long pc-only">
					<a href="https://stage-model.com/casting/">
						<p class="noto"><span>お仕事の依頼はこちら</span></p>
					</a>
				</div>
			</div>
			<div class="txt">
				<h2 class="name noto">${esc(v("f_name"))}<span class="mont">${esc(v("f_roma"))}</span></h2>
				<div class="dlwrap noto">
					<dl>
						<dt>生年月日</dt>
						<dd>${esc(v("f_birth"))}</dd>
					</dl>
					<dl>
						<dt>スリーサイズ</dt>
						<dd>B:${esc(v("f_b"))} W:${esc(v("f_w"))} H:${esc(v("f_h"))}</dd>
					</dl>
					<dl>
						<dt>身長</dt>
						<dd>${esc(v("f_height"))}</dd>
					</dl>
					<dl>
						<dt>靴のサイズ</dt>
						<dd>${esc(v("f_shoe"))}</dd>
					</dl>${fromDl}${hobbyDl}${skillDl}${licenseDl}
				</div>
				<div class="profile_info section_cmn">
					<div class="inner">
						<div class="paper">
							<h2 class="ttl_cmn noto"><em class="mont">INFORMATION</em>出演情報</h2>
							<div class="dlwrap noto">
${creditDls}
							</div>
						</div>
					</div>
				</div>
			</div>
		</div>
		<div class="btn_color bg_db2 center long sp-only">
			<a href="https://stage-model.com/casting/">
				<p class="noto"><span>お仕事の依頼はこちら</span></p>
			</a>
		</div>
	</div>
</div>

<style>
.slick-mainview img {
	width: 90%;
	margin: 0 auto;
}
</style>`;
}

$("gen").addEventListener("click",()=>{$("output").value=buildHTML();});
$("copy").addEventListener("click",async()=>{
  const t=$("output").value; if(!t){return;}
  try{await navigator.clipboard.writeText(t);showToast("コピーしました");}
  catch{$("output").select();document.execCommand("copy");showToast("コピーしました");}
});
$("download").addEventListener("click",()=>{
  const t=$("output").value;if(!t)return;
  const name=($("f_name").value||"profile").replace(/\s+/g,"")+".html";
  const blob=new Blob([t],{type:"text/html"});
  const a=document.createElement("a");a.href=URL.createObjectURL(blob);a.download=name;a.click();
});
$("clear").addEventListener("click",()=>{
  document.querySelectorAll("input,textarea").forEach(el=>el.value="");
  $("rawtext").textContent="（まだPDFを読み込んでいません）";
  setStatus("");
  initDate();
});

function showToast(msg){const el=$("toast");el.textContent=msg;el.classList.add("show");setTimeout(()=>el.classList.remove("show"),1600);}
</script>
</body>
</html>
