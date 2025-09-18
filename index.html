<!doctype html>
<html lang="ar">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>VideoBot — مولد فيديو من الصور (بدون سيرفر)</title>
<style>
  body{font-family:Arial, Helvetica, sans-serif;direction:rtl;text-align:right;padding:18px;background:#f6f7fb;color:#111}
  h1{font-size:20px}
  .box{background:#fff;border-radius:8px;padding:12px;margin:10px 0;box-shadow:0 2px 8px rgba(20,20,40,0.06)}
  input,button,select{padding:8px;border-radius:6px;border:1px solid #ddd;font-size:14px}
  .images-list{display:flex;flex-wrap:wrap;gap:8px;margin-top:8px}
  .img-thumb{width:100px;height:64px;object-fit:cover;border-radius:6px;border:1px solid #ccc}
  label{display:block;margin-bottom:6px}
  .row{display:flex;gap:8px;align-items:center}
  .center{display:flex;justify-content:center}
  #log{white-space:pre-wrap;font-size:13px;color:#333;margin-top:8px}
</style>
</head>
<body>
  <h1>🎬 VideoBot — صنع فيديو من الصور والموسيقى (بدون سيرفر)</h1>

  <div class="box">
    <label>1. ارفع صور (ترتيب الرفع هو ترتيب العرض)</label>
    <input id="inputImages" type="file" accept="image/*" multiple />
    <div class="images-list" id="thumbs"></div>
  </div>

  <div class="box">
    <label>2. (اختياري) ارفع ملف موسيقى</label>
    <input id="inputAudio" type="file" accept="audio/*" />
  </div>

  <div class="box">
    <label>3. إعدادات الفيديو</label>
    <div class="row">
      <label style="min-width:130px">مدة كل صورة (ثواني):</label>
      <input id="dur" type="number" value="3" min="0.5" step="0.5" style="width:90px"/>
      <label style="min-width:140px;margin-left:10px">دقة الفيديو:</label>
      <select id="res">
        <option value="1280x720">1280 × 720</option>
        <option value="854x480">854 × 480</option>
        <option value="1920x1080">1920 × 1080 (ثقيل)</option>
      </select>
    </div>

    <div style="margin-top:8px">
      <label>نص ثابت يظهر في أسفل الشاشة (اختياري)</label>
      <input id="overlayText" type="text" placeholder="اكتب نص يظهر فوق كل شريحة (اختياري)" style="width:100%"/>
    </div>
  </div>

  <div class="box center">
    <button id="startBtn">▶️ توليد الفيديو (ابدأ)</button>
    <button id="stopBtn" disabled style="margin-left:8px">⏹ إيقاف</button>
  </div>

  <div class="box">
    <label>نتيجة / تحميل</label>
    <div id="result"></div>
    <div id="log"></div>
  </div>

  <canvas id="hiddenCanvas" style="display:none"></canvas>

<script>
(async()=> {
  const inputImages = document.getElementById('inputImages');
  const inputAudio = document.getElementById('inputAudio');
  const thumbs = document.getElementById('thumbs');
  const durEl = document.getElementById('dur');
  const resEl = document.getElementById('res');
  const overlayTextEl = document.getElementById('overlayText');
  const startBtn = document.getElementById('startBtn');
  const stopBtn = document.getElementById('stopBtn');
  const result = document.getElementById('result');
  const log = document.getElementById('log');
  const canvas = document.getElementById('hiddenCanvas');

  let imageFiles = [];
  let audioFile = null;
  let recorder = null;

  function logMsg(s){ log.textContent += s + "\\n"; log.scrollTop = 9999; }

  inputImages.addEventListener('change', e=>{
    imageFiles = Array.from(e.target.files || []);
    thumbs.innerHTML = '';
    imageFiles.forEach(f=>{
      const url = URL.createObjectURL(f);
      const img = document.createElement('img');
      img.src = url;
      img.className = 'img-thumb';
      thumbs.appendChild(img);
    });
    logMsg('تم تحميل ' + imageFiles.length + ' صورة');
  });

  inputAudio.addEventListener('change', e=>{
    audioFile = e.target.files[0] || null;
    logMsg(audioFile ? ('تم تحميل موسيقى: ' + audioFile.name) : 'لا موسيقى');
  });

  function parseRes(val){
    const [w,h] = val.split('x').map(n=>parseInt(n));
    return {w,h};
  }

  startBtn.addEventListener('click', async ()=>{
    if(!imageFiles.length){ alert('يرجى رفع صور أولاً'); return; }
    startBtn.disabled = true;
    stopBtn.disabled = false;
    result.innerHTML = '';
    log.textContent = '';

    const durationPer = Math.max(0.5, parseFloat(durEl.value) || 3);
    const {w,h} = parseRes(resEl.value);
    canvas.width = w; canvas.height = h;
    const ctx = canvas.getContext('2d');

    // نجهز عناصر الصور
    const imgs = await Promise.all(imageFiles.map(f=>loadImageFromFile(f)));
    const overlayText = overlayTextEl.value || '';

    // إعداد الـ audio element
    let audioEl = null;
    let audioStream = null;
    if(audioFile){
      audioEl = new Audio(URL.createObjectURL(audioFile));
      audioEl.crossOrigin = "anonymous";
      audioEl.loop = false;
      try{
        // audioElement.captureStream works in many browsers
        audioStream = audioEl.captureStream ? audioEl.captureStream() : null;
      }catch(e){ audioStream = null; }
    }

    // stream من الـ canvas (video)
    const canvasStream = canvas.captureStream(30); // fps 30
    let mixedStream = canvasStream;
    if(audioStream && audioStream.getAudioTracks().length){
      const tracks = [
        ...canvasStream.getVideoTracks(),
        ...audioStream.getAudioTracks()
      ];
      mixedStream = new MediaStream(tracks);
    } else if(audioEl){
      // محاولة بديلة: إنشاء AudioContext لالتقاط صوت
      try{
        const ac = new (window.AudioContext||window.webkitAudioContext)();
        const src = ac.createMediaElementSource(audioEl);
        const dest = ac.createMediaStreamDestination();
        src.connect(dest);
        src.connect(ac.destination); // كي تسمع أثناء التسجيل
        const tracks = [...canvasStream.getVideoTracks(), ...dest.stream.getAudioTracks()];
        mixedStream = new MediaStream(tracks);
      }catch(e){
        console.warn('تعذر مزج الصوت بشكل مثالي:', e);
      }
    }

    // MediaRecorder
    const options = {mimeType: 'video/webm; codecs=vp9,opus'};
    try{
      recorder = new MediaRecorder(mixedStream, options);
    }catch(e){
      try{ recorder = new MediaRecorder(mixedStream, {mimeType:'video/webm'}); }
      catch(err){ alert('المتصفح لا يدعم MediaRecorder لالتقاط الفيديو؛ استخدم Chrome/Edge/Firefox حديث'); startBtn.disabled=false; stopBtn.disabled=true; return; }
    }

    const chunks = [];
    recorder.ondataavailable = (ev)=>{ if(ev.data && ev.data.size) chunks.push(ev.data); };
    recorder.onstop = async ()=>{
      const blob = new Blob(chunks, {type: 'video/webm'});
      const url = URL.createObjectURL(blob);
      const a = document.createElement('a');
      a.href = url; a.download = 'video_bot_'+Date.now()+'.webm';
      a.textContent = '⬇ تحميل الفيديو (WebM)';
      result.innerHTML = '';
      result.appendChild(a);
      // إضافة معاينة
      const v = document.createElement('video');
      v.controls = true; v.src = url; v.style.maxWidth='100%'; v.style.display='block'; v.style.marginTop='8px';
      result.appendChild(v);
      logMsg('اكتمل التصدير. حجم الملف تقريبي: ' + Math.round(blob.size/1024) + ' KB');
      startBtn.disabled = false;
      stopBtn.disabled = true;
    };

    recorder.start(200); // دفعات كل 200ms
    logMsg('بدأ التسجيل...');

    // تشغيل الموسيقى (إذا وُجد)
    if(audioEl){
      try { await audioEl.play(); } catch(e){ console.warn('الصوت قد لا يبدأ تلقائياً بسبب سياسات المتصفح'); }
    }

    // رسم الشرائح على الكانفس بالتوقيت
    let stopped = false;
    stopBtn.onclick = ()=>{ stopped = true; if(audioEl){ audioEl.pause(); } recorder && recorder.state==='recording' && recorder.stop(); };

    for(let i=0; i<imgs.length && !stopped; i++){
      const img = imgs[i];
      // رسم خلفية سوداء
      ctx.fillStyle = '#000'; ctx.fillRect(0,0,w,h);
      // حساب مناسب للصورة (contain)
      let iw = img.width, ih = img.height;
      const scale = Math.min(w/iw, h/ih);
      const dw = iw*scale, dh = ih*scale;
      const dx = (w - dw)/2, dy = (h - dh)/2;
      ctx.drawImage(img, dx, dy, dw, dh);

      // نص شفاف أعلى/أسفل إن وُجد
      if(overlayText){
        ctx.fillStyle = 'rgba(0,0,0,0.45)';
        ctx.fillRect(0, h - 80, w, 80);
        ctx.fillStyle = '#fff';
        ctx.font = Math.max(18, Math.floor(h*0.04)) + 'px Arial';
        ctx.textAlign = 'center';
        ctx.fillText(overlayText, w/2, h - 30);
      }

      // ننتظر مدة الصورة مع تحديث الإطار (نرسم إطار كل 100ms لضمان سلاسة)
      const totalMs = Math.max(500, Math.round(durationPer*1000));
      const stepMs = 100;
      const steps = Math.ceil(totalMs/stepMs);
      for(let s=0;s<steps && !stopped;s++){
        // يمكن إضافة حركة بسيطة هنا (تكبير بسيط)
        await wait(stepMs);
      }
      logMsg('انتهت شريحة ' + (i+1) + ' / ' + imgs.length);
    }

    // إذا انتهت الشرائح بنوقف التسجيل (نقطع الصوت بعد قليل لكي يكتمل الملف)
    if(!stopped){
      // انتظر 500ms ثم أوقف
      await wait(500);
      if(audioEl){ audioEl.pause(); }
      recorder && recorder.state==='recording' && recorder.stop();
    }
  });

  stopBtn.addEventListener('click', ()=>{
    if(recorder && recorder.state==='recording'){
      recorder.stop();
    }
    startBtn.disabled = false;
    stopBtn.disabled = true;
  });

  function wait(ms){ return new Promise(res=>setTimeout(res,ms)); }

  function loadImageFromFile(file){
    return new Promise((res,rej)=>{
      const img = new Image();
      img.onload = ()=>res(img);
      img.onerror = rej;
      img.src = URL.createObjectURL(file);
    });
  }

})();
</script>
</body>
</html>
