# taxiqr
taxiqrrr
<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>계좌/페이팔 정보 — 복사 & QR</title>
  <style>
    body{font-family: -apple-system,BlinkMacSystemFont,'Segoe UI',Roboto,'Noto Sans KR',Arial; background:#f6f7fb; color:#111; padding:24px}
    .card{background:#fff;border-radius:10px;box-shadow:0 6px 18px rgba(20,20,40,0.06);padding:18px;margin-bottom:16px;display:flex;gap:16px;align-items:center}
    .meta{flex:1}
    h3{margin:0 0 6px;font-size:18px}
    .label{font-size:13px;color:#666;margin-bottom:6px}
    .value-row{display:flex;gap:8px;align-items:center}
    input.value{font-size:16px;padding:10px;border:1px solid #e6e8ef;border-radius:8px;min-width:220px}
    button.copy{padding:10px 12px;border-radius:8px;border:0;background:#2563eb;color:#fff;cursor:pointer}
    .hint{font-size:13px;color:#666;margin-top:8px}
    .qr{width:110px;height:110px;flex-shrink:0;border-radius:8px;border:1px solid #eee;display:flex;align-items:center;justify-content:center;background:#fff}
    .toaster{position:fixed;left:50%;transform:translateX(-50%);bottom:24px;background:#111;color:#fff;padding:10px 14px;border-radius:8px;opacity:0;transition:opacity .18s;pointer-events:none}
    .toaster.show{opacity:1;pointer-events:auto}
    .controls{display:flex;gap:8px;align-items:center;margin-bottom:12px}
    .small{font-size:13px;color:#444}
    .btn-ghost{padding:8px 10px;border-radius:8px;border:1px solid #d1d5db;background:transparent;cursor:pointer}
  </style>
</head>
<body>
  <h2>QR 옵션: 텍스트 QR / 페이지 URL QR (택시용)</h2>
  <div class="controls">
    <div class="small">웹페이지 URL (호스팅 후 여기에 넣으세요):</div>
    <input id="page-url" placeholder="https://your-domain.example/qrpage.html" style="padding:8px;border-radius:8px;border:1px solid #e6e8ef;min-width:320px" />
    <button class="btn-ghost" id="apply-url">URL로 QR 만들기</button>
    <button class="btn-ghost" id="use-text">텍스트 QR로 복원</button>
  </div>

  <div class="card">
    <div class="meta">
      <h3>■ 카카오뱅크 — 김태훈</h3>
      <div class="label">계좌번호</div>
      <div class="value-row">
        <input class="value" id="kb-account" readonly value="3333015714391" />
        <button class="copy" data-target="kb-account">복사하기</button>
      </div>
      <div class="hint">(복사해서 붙여넣기)</div>
    </div>
    <div class="qr">
      <img id="qr-kb" alt="QR: 카카오뱅크 계좌" src="" style="width:100%;height:100%;object-fit:cover;border-radius:8px" />
    </div>
  </div>

  <div class="card">
    <div class="meta">
      <h3>■ PayPal</h3>
      <div class="label">email</div>
      <div class="value-row">
        <input class="value" id="pp-email" readonly value="hoon3ga@naver.com" />
        <button class="copy" data-target="pp-email">Copy</button>
      </div>
      <div class="hint">(Copy and paste)</div>
    </div>
    <div class="qr">
      <img id="qr-pp" alt="QR: PayPal 이메일" src="" style="width:100%;height:100%;object-fit:cover;border-radius:8px" />
    </div>
  </div>

  <div class="toaster" id="toaster">복사되었습니다</div>

  <script>
    // QR 이미지 생성: 외부 API 사용
    function makeQrSrc(data){
      return 'https://api.qrserver.com/v1/create-qr-code/?size=420x420&data=' + encodeURIComponent(data);
    }

    // 기본: 텍스트 QR (계좌/이메일 텍스트 자체를 QR로 인코딩)
    const kbText = '카카오뱅크 김태훈\n계좌번호: 3333015714391';
    const ppText = 'PayPal\nemail: hoon3ga@naver.com';

    const elKb = document.getElementById('qr-kb');
    const elPp = document.getElementById('qr-pp');

    function setTextQr(){
      elKb.src = makeQrSrc(kbText);
      elPp.src = makeQrSrc(ppText);
    }

    // URL QR: 페이지 URL을 입력하면 두 QR 모두 그 URL로 변경 (스캔 시 해당 페이지로 이동)
    function setUrlQr(url){
      if(!url) return;
      elKb.src = makeQrSrc(url);
      elPp.src = makeQrSrc(url);
    }

    // 초기화: 텍스트 QR 표시
    setTextQr();

    // 복사 버튼 로직
    function showToast(text){
      const t = document.getElementById('toaster');
      t.textContent = text;
      t.classList.add('show');
      clearTimeout(t._timer);
      t._timer = setTimeout(()=> t.classList.remove('show'), 1600);
    }

    async function copyText(text){
      try{
        if(navigator.clipboard && navigator.clipboard.writeText){
          await navigator.clipboard.writeText(text);
        } else {
          const ta = document.createElement('textarea');
          ta.value = text; document.body.appendChild(ta);
          ta.select(); document.execCommand('copy');
          document.body.removeChild(ta);
        }
        showToast('복사되었습니다');
        return true;
      }catch(e){
        showToast('복사에 실패했습니다');
        return false;
      }
    }

    document.querySelectorAll('button.copy').forEach(btn=>{
      btn.addEventListener('click', async ()=>{
        const id = btn.getAttribute('data-target');
        const val = document.getElementById(id).value.trim();
        await copyText(val);
      });
    });

    // 컨트롤 버튼
    document.getElementById('apply-url').addEventListener('click', ()=>{
      const url = document.getElementById('page-url').value.trim();
      if(!url){ showToast('URL을 입력하세요'); return; }
      setUrlQr(url);
      showToast('QR이 페이지 URL로 변경되었습니다');
    });
    document.getElementById('use-text').addEventListener('click', ()=>{ setTextQr(); showToast('텍스트 QR로 복원되었습니다'); });

    // 안내: 페이지를 호스팅한 뒤 '웹페이지 URL' 칸에 올린 페이지의 URL을 넣으면
    // 스캔할 때 바로 해당 페이지로 이동합니다. (택시 고객이 스캔하면 결제 정보 페이지로 연결 가능)
  </script>
</body>
</html>
