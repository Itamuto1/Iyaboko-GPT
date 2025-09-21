<div style="max-width:600px;margin:20px auto;padding:20px;background:#0b0c10;color:#e0e0e0;border-radius:10px;box-shadow:0 0 15px rgba(212,175,55,0.3);">
  <h2 style="text-align:center;color:#d4af37;">✨ Totaphysics AI ✨</h2>
  <textarea id="totaphysics-input" placeholder="Ask Totaphysics AI..." rows="4"
    style="width:100%;padding:10px;border-radius:5px;border:1px solid #444;background:#1a1a1a;color:#fff;"></textarea>
  <button onclick="askTotaphysics()" 
    style="margin-top:10px;width:100%;padding:10px;background:#d4af37;color:#000;border:none;border-radius:5px;font-weight:bold;cursor:pointer;">
    Ask
  </button>
  <div id="totaphysics-response" 
    style="margin-top:20px;white-space:pre-wrap;background:#111;padding:15px;border-radius:5px;border:1px solid #333;min-height:120px;line-height:1.5;">
    ⏳ Waiting for your question...
  </div>
</div>

<script>
async function askTotaphysics() {
  const input = document.getElementById("totaphysics-input").value.trim();
  const responseBox = document.getElementById("totaphysics-response");
  if (!input) {
    responseBox.innerHTML = "⚠️ Please type a question first.";
    return;
  }
  responseBox.innerHTML = "⏳ Thinking...";

  try {
    const res = await fetch("https://ai.iyaboko.org/api/chat", {
      method: "POST",
      body: JSON.stringify({ message: input })
    });

    const reader = res.body.getReader();
    const decoder = new TextDecoder("utf-8");
    responseBox.innerHTML = "";

    while (true) {
      const { done, value } = await reader.read();
      if (done) break;
      const chunk = decoder.decode(value, { stream: true });
      responseBox.innerHTML += chunk;
      responseBox.scrollTop = responseBox.scrollHeight;
    }
  } catch (err) {
    responseBox.innerHTML = "❌ Error: " + err.message;
  }
}
</script>
