<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>위험기계·기구 자율점검 – 지게차 자가진단 (표 + 대표 사진 고정)</title>
  <style>
    :root{--bg:#f8fafc;--card:#ffffff;--muted:#64748b;--line:#cfd8e3;--ink:#0f172a;--blue:#0ea5e9;--blue-d:#0369a1;--orange:#f59e0b;--orange-d:#c2410c}
    html,body{height:100%}
    body{margin:0;background:var(--bg);color:var(--ink);font-family:system-ui,Segoe UI,Roboto,Apple SD Gothic Neo,Malgun Gothic,sans-serif}
    .container{max-width:860px;margin:0 auto;padding:12px}
    .head{display:flex;justify-content:space-between;align-items:center;gap:8px;margin-bottom:10px}
    .title{font-size:22px;font-weight:800;margin:0}
    .btns{display:flex;gap:8px;flex-wrap:wrap}
    .btn{padding:8px 10px;border-radius:10px;border:1px solid #d1d5db;background:#fff;cursor:pointer;font-size:14px}
    .card{background:var(--card);border:1px solid var(--line);border-radius:14px;box-shadow:0 4px 14px rgba(0,0,0,.06);padding:12px;margin-bottom:10px}
    .grid2{display:grid;grid-template-columns:1fr 1fr;gap:10px}
    .row{display:grid;grid-template-columns:100px 1fr;gap:8px;align-items:center}
    textarea,input[type="text"],input[type="datetime-local"]{padding:10px;border:1px solid #d1d5db;border-radius:10px;width:100%;box-sizing:border-box;font-size:16px;background:#fff}
    .note{font-size:12px;color:var(--muted)}

    /* ===== 표 레이아웃 ===== */
    .check-table{width:100%;border-collapse:collapse;background:#fff;border-radius:12px;overflow:hidden}
    .check-table th,.check-table td{border:1px solid var(--line);padding:8px;vertical-align:middle}
    .check-table th{background:#eef3f8;font-weight:800;text-align:center}
    .col-no{width:48px;text-align:center}
    .col-item{width:240px}
    .col-method{width:auto}
    .col-opt{width:70px;text-align:center}
    .itemcell{display:grid;grid-template-columns:96px 1fr;gap:8px;align-items:center}
    .thumb{width:96px;height:72px;border:1px solid var(--line);border-radius:8px;object-fit:cover;background:#f1f5f9}
    .itemtext{display:flex;flex-direction:column;gap:2px}
    .label{font-weight:700}
    .method{font-size:12px;color:var(--muted);line-height:1.45;white-space:pre-line}
    .opt-btn{display:inline-flex;align-items:center;justify-content:center;padding:6px 10px;border-radius:10px;border:1px solid #d1d5db;background:#fff;font-size:13px;width:100%}
    .ok.selected{background:var(--blue);color:#fff;border-color:var(--blue-d)}
    .bad.selected{background:var(--orange);color:#fff;border-color:var(--orange-d)}
    .table-wrap{overflow-x:auto}
    .check-table{min-width:720px}

    /* ===== 대표 사진(고정) ===== */
    .hero-head{display:flex;justify-content:space-between;align-items:center;margin-bottom:8px}
    .hero-title{font-weight:800}
    .hero-box{display:flex;align-items:center;justify-content:center;height:260px;border:1px solid var(--line);border-radius:12px;background:#fafafa}
    .hero-img{max-width:100%;max-height:100%;object-fit:contain;border-radius:10px}

    .overlay{position:fixed;inset:0;background:rgba(255,255,255,.75);backdrop-filter:saturate(140%) blur(1px);display:none;align-items:center;justify-content:center;z-index:9999;font-weight:700}
    .spinner{width:28px;height:28px;border:3px solid #94a3b8;border-top-color:transparent;border-radius:50%;animation:spin 1s linear infinite;margin-right:10px}
    @keyframes spin{to{transform:rotate(360deg)}}
  </style>
</head>
<body>
  <div class="container">
    <header class="head">
      <h1 class="title">위험기계·기구 자율점검 – 지게차 자가진단</h1>
      <div class="btns">
        <span id="diag" class="note" style="display:none"></span>
        <button id="pngBtn" class="btn">🖼 이미지</button>
        <button id="resetBtn" class="btn">↺ 초기화</button>
      </div>
    </header>

    <div id="capture">
      <!-- 기본 정보 -->
      <section class="card">
        <h2 style="font-size:18px;font-weight:700;margin:0 0 8px">기본 정보</h2>
        <div class="grid2">
          <div class="row"><label for="date">점검 일시</label><input id="date" type="datetime-local" /></div>
          <div class="row"><label for="org">소속</label><input id="org" type="text" placeholder="예) 생산1팀" /></div>
          <div class="row"><label for="site" id="siteLabel">점검 장소</label><input id="site" type="text" placeholder="예) 2공장 A라인" /></div>
          <div class="row"><label for="inspector">점검자</label><input id="inspector" type="text" placeholder="성명" /></div>
          <div class="row"><label for="equip">장비 구분</label><input id="equip" type="text" value="지게차" /></div>
          <div class="row"><label for="equipId">장비 번호 </label><input id="equipId" type="text" placeholder="예) 00라0000" /></div>
        </div>
      </section>

      <!-- ✅ 대표 사진(고정 이미지) -->
      <section class="card">
        <div class="hero-head"><div class="hero-title">대표 사진</div></div>
        <div class="hero-box">
          <img id="heroImg" class="hero-img" alt="대표 사진" />
        </div>
      </section>

      <!-- 표 형태의 자가진단 -->
      <section class="card table-wrap">
        <table class="check-table" id="checkTable">
          <thead>
            <tr>
              <th class="col-no">No</th>
              <th class="col-item">점검항목</th>
              <th class="col-method">점 검 방 법</th>
              <th class="col-opt">양호</th>
              <th class="col-opt">불량</th>
            </tr>
          </thead>
          <tbody id="tblBody"></tbody>
        </table>

        <div class="grid2" style="margin-top:12px">
          <div>
            <div class="note" style="margin-bottom:6px">특이사항/비고</div>
            <textarea rows="5" id="remarks" placeholder="현장 특이사항 또는 사진 번호 기재"></textarea>
          </div>
          <div>
            <div class="note" style="margin-bottom:6px">조치 내용 (불량 발생 시 필수)</div>
            <textarea rows="5" id="action" placeholder="조치계획·완료일·담당자 등"></textarea>
          </div>
        </div>
      </section>
    </div>

    <div class="note" style="margin-top:12px">URL 예: <code>?equipId=FLT-01&site=2공장A라인&org=생산1팀&equip=지게차&siteLabel=설치장소</code></div>
  </div>

  <div id="overlay" class="overlay"><div class="spinner"></div><span>이미지 준비 중...</span></div>

  <script>
    const $ = (id)=>document.getElementById(id);

    /* 고정 대표 사진 소스
       - 프로젝트에 images/forklift-hero.jpg 파일을 두고 아래 경로를 교체하세요.
       - 외부 URL을 쓰면 CORS로 인해 캡처가 안될 수 있으니 같은 도메인을 권장합니다. */
    const HERO_SRC = '지게차.png';

    const PLACEHOLDER_THUMB = 'data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="192" height="144"><rect width="100%" height="100%" fill="%23eef2f7"/><text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle" font-family="sans-serif" font-size="12" fill="%2399a">참고사진</text></svg>';

    const checklistTemplate = [
      { key:'rear_cam',       label:'후방감시카메라',     method:'후방 감시카메라는 상시 전원이 켜져있고 모니터 등 작동에 문제가 없을 것', refImg:'후방감시카메라.png'  },
      { key:'controls_brake', label:'조종장치, 제동장치', method:'조종장치, 클러치, 브레이크 등은 정상작동되고 좌석안전띠가 부착될 것',             refImg: '조종장치.png' },
      { key:'mirror_alarm',   label:'후사경, 후진경보기', method:'후사경은 정상 부착되고 후진 경보장치는 정상적으로 작동될 것',                    refImg: '후사경.png' },
      { key:'hydraulic_cyl',  label:'유압장치 및 실린더', method:'유압모터ㆍ실린더ㆍ배관 등에 누유 및 손상, 마모 등이 없을 것',                     refImg:'유압장치.png' },
      { key:'tire',           label:'차륜(타이어)',       method:'차륜(타이어)의 균열 및 변형이 없고, 체결상태가 양호할 것',                         refImg:'차륜.png'},
      { key:'guard_rail',     label:'방호울',             method:'방호울은 탈락되지 않고 견고히 설치할 것',                                         refImg: '방호울.png' },
      { key:'fork_handling',  label:'포크 (하역장치)',     method:'변형 및 균열이 없고, 고정핀은 견고하게 체결되어 있을 것',                          refImg:'포크.jpg' },
      { key:'lamps',          label:'각종 등화류',        method:'전조ㆍ후미ㆍ방향지시ㆍ경보등의 기능은 정상작동 될 것',                               refImg: '등화류.png' },
      { key:'head_backrest',  label:'헤드가드, 백레스트', method:'헤드가드와 백레스트는 정상적으로 설치되고 변형 등의 이상이 없을 것',                 refImg: '헤드가드.png' },
      { key:'extinguisher',   label:'소화기',             method:'소화기(거치대포함)가 설치되어 있을 것, 사용연수가 지나지 않았는지 확인할 것',        refImg: '소화기.jpg' },
      { key:'etc',            label:'기타',               method:'승강용 발판상태, 타이어 타이거 마크 표시 확인할 것',                              refImg: '타이거.png' }
    ];

    const state = {
      date:'', org:'', site:'', inspector:'', equip:'지게차', equipId:'',
      checks: Object.fromEntries(checklistTemplate.map(i=>[i.key,''])),
      remarks:'', action:''
    };

    function loadParams(){
      const p = new URLSearchParams(location.search); const now=new Date();
      state.date = p.get('date') || new Date(now.getTime()-now.getTimezoneOffset()*60000).toISOString().slice(0,16);
      state.org = p.get('org') || ''; state.site=p.get('site')||''; state.inspector=p.get('inspector')||'';
      state.equip = p.get('equip') || '지게차'; state.equipId=p.get('equipId')||'';
      const lbl=p.get('siteLabel'); if(lbl) $('siteLabel').textContent=lbl;
    }

    function applySelection(key){
      const ok=document.querySelector('button.ok[data-key="'+key+'"]');
      const bad=document.querySelector('button.bad[data-key="'+key+'"]');
      if(!ok||!bad) return;
      ok.classList.toggle('selected', state.checks[key]==='양호');
      bad.classList.toggle('selected', state.checks[key]==='불량');
    }

    function renderTable(){
      $('date').value=state.date; $('org').value=state.org; $('site').value=state.site;
      $('inspector').value=state.inspector; $('equip').value=state.equip; $('equipId').value=state.equipId;

      // 고정 대표 사진 설정 (로드 실패 시 alt 표시)
      const hero=$('heroImg'); hero.src=HERO_SRC; hero.onerror=()=>{ hero.src='data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="800" height="520"><rect width="100%" height="100%" fill="%23f1f5f9"/><text x="50%" y="50%" dominant-baseline="middle" text-anchor="middle" font-family="sans-serif" font-size="16" fill="%2399a">대표 사진을 불러오지 못했습니다. images/forklift-hero.jpg 파일을 확인하세요.</text></svg>'; };

      const body=$('tblBody'); body.innerHTML='';
      checklistTemplate.forEach((row,i)=>{
        const tr=document.createElement('tr');
        tr.innerHTML =
          '<td class="col-no">'+(i+1)+'</td>'+
          '<td class="col-item">'+
            '<div class="itemcell">'+
              '<img class="thumb" src="'+row.refImg+'" alt="'+row.label+'">'+
              '<div class="itemtext"><div class="label">'+row.label+'</div></div>'+
            '</div>'+
          '</td>'+
          '<td class="col-method"><div class="method">'+row.method+'</div></td>'+
          '<td class="col-opt"><button class="opt-btn ok"  data-key="'+row.key+'" data-val="양호">양호</button></td>'+
          '<td class="col-opt"><button class="opt-btn bad" data-key="'+row.key+'" data-val="불량">불량</button></td>';
        body.appendChild(tr);
      });

      body.addEventListener('click',(e)=>{
        const b=e.target.closest('button'); if(!b) return; const key=b.getAttribute('data-key'), val=b.getAttribute('data-val');
        state.checks[key]=val; applySelection(key);
      });

      checklistTemplate.forEach(r=>applySelection(r.key));
      $('remarks').value=state.remarks; $('action').value=state.action;
    }

    function bindInputs(){
      $('date').addEventListener('change', e=>state.date=e.target.value);
      $('org').addEventListener('input', e=>state.org=e.target.value);
      $('site').addEventListener('input', e=>state.site=e.target.value);
      $('inspector').addEventListener('input', e=>state.inspector=e.target.value);
      $('equip').addEventListener('input', e=>state.equip=e.target.value);
      $('equipId').addEventListener('input', e=>state.equipId=e.target.value);
      $('remarks').addEventListener('input', e=>state.remarks=e.target.value);
      $('action').addEventListener('input', e=>state.action=e.target.value);
      $('resetBtn').addEventListener('click', resetForm);
      $('pngBtn').addEventListener('click', downloadPNG);
    }

    function resetForm(){
      const now=new Date();
      state.date=new Date(now.getTime()-now.getTimezoneOffset()*60000).toISOString().slice(0,16);
      state.org=state.site=state.inspector=state.equipId='';
      state.equip='지게차';
      state.checks = Object.fromEntries(checklistTemplate.map(i=>[i.key,'']));
      state.remarks=state.action='';
      renderTable();
    }

    /* ===== html2canvas & PNG 저장 ===== */
    function ensureHtml2Canvas(cb){
      if(window.html2canvas){ cb(); return; }
      const diag=$('diag'); diag.style.display='inline'; diag.textContent='이미지 라이브러리 로딩 중...';
      const srcs=[
        'https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js',
        'https://unpkg.com/html2canvas@1.4.1/dist/html2canvas.min.js',
        'https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js'
      ];
      (function load(i){
        if(i>=srcs.length){ diag.textContent='이미지 로딩 실패'; return; }
        const s=document.createElement('script'); s.src=srcs[i]; s.onload=()=>{ diag.textContent=''; diag.style.display='none'; cb(); }; s.onerror=()=>load(i+1); document.head.appendChild(s);
      })(0);
    }
    function showBusy(msg){ const o=$('overlay'); o.style.display='flex'; o.querySelector('span').textContent=msg||'처리 중...'; }
    function hideBusy(){ $('overlay').style.display='none'; }
    function downloadPNG(){
      ensureHtml2Canvas(function(){
        showBusy('이미지 준비 중...');
        const el=$('capture'); const prev=el.style.backgroundColor; el.style.backgroundColor='#ffffff';
        const scale=(window.devicePixelRatio>1)?2:1.5;
        html2canvas(el,{scale:scale,useCORS:true,backgroundColor:'#ffffff'}).then((canvas)=>{
          const data=canvas.toDataURL('image/png');
          const name='지게차_자가진단_'+(($('equipId').value)||'장비')+'_'+(($('date').value)||'').replace(/[:T]/g,'-')+'.png';
          const a=document.createElement('a'); a.href=data; a.download=name;
          if(typeof a.download==='undefined'){ window.open(data,'_blank'); } else { document.body.appendChild(a); a.click(); a.remove(); }
        }).catch((e)=>alert('이미지 생성 실패: '+e.message)).finally(()=>{ el.style.backgroundColor=prev||''; hideBusy(); });
      });
    }

    function start(){ loadParams(); renderTable(); bindInputs(); ensureHtml2Canvas(()=>{}); }
    if(document.readyState==='loading') document.addEventListener('DOMContentLoaded', start); else start();

    // 간단 테스트
    (function(){ try{
      setTimeout(()=>{
        console.assert(document.querySelectorAll('#tblBody tr').length===11,'행 수가 11이 아님');
        console.assert(!!document.getElementById('heroImg'), '대표 사진 요소 누락');
      },0);
    }catch(e){} })();
  </script>
</body>
</html>
