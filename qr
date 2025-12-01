<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>계좌/PayPal 복사 & QR</title>
  <style>
    body{font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Noto Sans KR", "Apple SD Gothic Neo", "Malgun Gothic", sans-serif; background:#f7f7f8; color:#111; padding:20px;}
    .card{background:#fff; border-radius:12px; box-shadow:0 6px 18px rgba(16,24,40,0.06); padding:18px; max-width:520px; margin:18px auto;}
    h1{font-size:18px; margin:0 0 12px;}
    .row{display:flex; gap:14px; align-items:center;}
    .info{flex:1;}
    .label{font-size:13px; color:#666; margin-bottom:6px;}
    .value{font-size:16px; font-weight:600; word-break:break-all;}
    button.copy{margin-top:10px; padding:8px 12px; border:0; border-radius:8px; cursor:pointer; background:#111; color:#fff; font-weight:600;}
    .qr{width:120px; height:120px; border-radius:8px; background:#fff; padding:6px; box-sizing:border-box;}
    .hint{font-size:12px; color:#666; margin-top:8px;}
    .toast{position:fixed; right:20px; bottom:20px; background:#111; color:#fff; padding:10px 14px; border-radius:10px; opacity:0; transform:translateY(12px); transition:all .25s; pointer-events:none;}
    .toast.show{opacity:1; transform:translateY(0); pointer-events:auto;}
    .small{font-size:13px; color:#444;}
    footer{margin-top:18px; font-size:12px; color:#777; text-align:center;}
    /* 모바일도 보기 좋게 */
    @media (max-width:480px){
      .row{flex-direction:row; gap:10px;}
      .qr{width:96px; height:96px;}
    }
  </style>
</head>
<body>

  <div class="card">
    <h1>■ 카카오뱅크 — 김태훈</h1>
    <div class="row">
      <div class="info">
        <div class="label">계좌번호</div>
        <div id="kb-account" class="value">3333015714391</div>
        <button class="copy" data-copy="3333015714391">계좌번호 복사하기</button>
        <div class="hint">복사한 값을 붙여넣기 하세요 (예: 인터넷뱅킹, 카카오톡 등)</div>
      </div>
      <div>
        <img id="qr-kb" class="qr" alt="KakaoBank QR">
      </div>
    </div>
  </div>

  <div class="card">
    <h1>■ PayPal</h1>
    <div class="row">
      <div class="info">
        <div class="label">Email</div>
        <div id="pp-email" class="value">hoon3ga@naver.com</div>
        <button class="copy" data-copy="hoon3ga@naver.com">이메일 복사하기</button>
        <div class="hint">PayPal 송금 시 이메일을 붙여넣기 하세요.</div>
      </div>
      <div>
        <img id="qr-pp" class="qr" alt="PayPal QR">
      </div>
    </div>
  </div>

  <footer>이 파일을 메모장에 붙여넣고 .html로 저장한 뒤 브라우저로 열면 됩니다.</footer>

  <div id="toast" class="toast">복사되었습니다!</div>

  <script>
    // QR 이미지 만들기 (Google Chart API 사용)
    function makeQR(text, size = 240){
      var d = encodeURIComponent(text);
      // Google Chart QR API: chart.googleapis.com/chart?cht=qr&chs=SIZExSIZE&chl=...
      // 여기서는 보안상 HTTPS 사용
      return "https://chart.googleapis.com/chart?cht=qr&chs=" + size + "x" + size + "&chl=" + d + "&chld=L|1";
    }

    // 초기값(이미 제공한 텍스트)
    const kbText = "KakaoBank 김태훈\n계좌번호: 3333015714391";
    const ppText = "PayPal\nEmail: hoon3ga@naver.com";

    document.getElementById("qr-kb").src = makeQR(kbText, 300);
    document.getElementById("qr-pp").src = makeQR(ppText, 300);

    // 복사 버튼 처리
    document.querySelectorAll('button.copy').forEach(btn=>{
      btn.addEventListener('click', async (e) => {
        const txt = btn.getAttribute('data-copy') || '';
        if (!txt) return;
        try {
          // navigator.clipboard 사용 (HTTPS 또는 로컬 파일에서 동작)
          await navigator.clipboard.writeText(txt);
          showToast("복사되었습니다!");
        } catch(err) {
          // 실패 시 대체: 텍스트영역 생성해서 복사
          const ta = document.createElement('textarea');
          ta.value = txt;
          document.body.appendChild(ta);
          ta.select();
          try {
            document.execCommand('copy');
            showToast("복사되었습니다!");
          } catch(e2) {
            showToast("복사에 실패했습니다. 수동으로 선택하여 복사하세요.");
          }
          document.body.removeChild(ta);
        }
      });
    });

    // 토스트(간단한 피드백)
    const toast = document.getElementById('toast');
    let toastTimer = null;
    function showToast(msg){
      toa
