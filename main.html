from flask import Flask, request, jsonify, render_template_string
from collections import defaultdict

app = Flask(__name__)

# Store signaling messages per room and per peer
rooms = defaultdict(lambda: {"caller": [], "callee": []})

HTML = '''
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>Simple WebRTC Call</title>
<style>
  body { font-family: Arial; background: #111; color: #fff; text-align: center; }
  video { width: 300px; margin: 10px; border-radius: 10px; background: black; }
  input, button { padding: 8px; margin: 5px; }
</style>
</head>
<body>
<h2>Simple WebRTC Call</h2>
<video id="localVideo" autoplay muted></video>
<video id="remoteVideo" autoplay></video>

<br/>
<input id="room" placeholder="Room Name" />
<button id="start">Start</button>
<button id="call" disabled>Call</button>

<script>
const startBtn = document.getElementById('start');
const callBtn = document.getElementById('call');
const roomInput = document.getElementById('room');
const localVideo = document.getElementById('localVideo');
const remoteVideo = document.getElementById('remoteVideo');

let localStream;
let pc;
let room;
let peer = null; // "caller" or "callee"

const config = { iceServers: [{ urls: 'stun:stun.l.google.com:19302' }] };

startBtn.onclick = async () => {
  room = roomInput.value.trim();
  if (!room) return alert('Enter a room name');

  localStream = await navigator.mediaDevices.getUserMedia({ video: true, audio: true });
  localVideo.srcObject = localStream;

  // Decide role: if first in room, caller; else callee
  const res = await fetch(`/signal/${room}/caller`);
  const msgs = await res.json();
  if (msgs.length === 0) {
    peer = "caller";
  } else {
    peer = "callee";
  }

  callBtn.disabled = false;
  startBtn.disabled = true;

  pollSignals();
};

callBtn.onclick = async () => {
  callBtn.disabled = true;

  pc = new RTCPeerConnection(config);

  localStream.getTracks().forEach(track => pc.addTrack(track, localStream));

  pc.onicecandidate = e => {
    if (e.candidate) {
      sendSignal({ ice: e.candidate });
    }
  };

  pc.ontrack = e => {
    remoteVideo.srcObject = e.streams[0];
  };

  if (peer === "caller") {
    const offer = await pc.createOffer();
    await pc.setLocalDescription(offer);
    await sendSignal({ sdp: pc.localDescription });
  }
};

async function sendSignal(data) {
  await fetch(`/signal/${room}/${peer}`, {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify(data)
  });
}

async function pollSignals() {
  while(true) {
    const res = await fetch(`/signal/${room}/${peer}`);
    const messages = await res.json();

    for (const msg of messages) {
      if (msg.sdp) {
        await pc.setRemoteDescription(new RTCSessionDescription(msg.sdp));
        if (msg.sdp.type === 'offer') {
          const answer = await pc.createAnswer();
          await pc.setLocalDescription(answer);
          await sendSignal({ sdp: pc.localDescription });
        }
      }
      if (msg.ice) {
        await pc.addIceCandidate(msg.ice);
      }
    }
    await new Promise(r => setTimeout(r, 2000)); // poll every 2 seconds
  }
}
</script>
</body>
</html>
'''

@app.route('/')
def index():
    return render_template_string(HTML)

@app.route('/signal/<room>/<peer>', methods=['POST'])
def post_signal(room, peer):
    data = request.json
    other_peer = "callee" if peer == "caller" else "caller"
    rooms[room][other_peer].append(data)
    return jsonify({"status": "ok"})

@app.route('/signal/<room>/<peer>', methods=['GET'])
def get_signal(room, peer):
    messages = rooms[room][peer]
    rooms[room][peer] = []  # clear after sending
    return jsonify(messages)

if __name__ == '__main__':
    app.run(debug=True)
