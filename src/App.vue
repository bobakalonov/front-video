<template>
  <div class="app-vue">
    <transition>
      <div class="form-modal" v-if="loginModal">
        <form
          @submit.prevent="login"
          class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4"
        >
          <div class="mb-4 text-xl">
            <label
              class="block text-gray-700 text-2xl font-bold mb-2"
              for="username"
            >
              Telefon
            </label>
            <input
              class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline"
              id="username"
              type="text"
              placeholder="Your Phone"
              v-model="phone"
            />
          </div>
          <div class="mb-6 text-xl">
            <label
              class="block text-gray-700 text-2xl font-bold mb-2"
              for="password"
            >
              Parol
            </label>
            <input
              class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline"
              id="password"
              type="password"
              v-model="password"
              placeholder="Parolni Kiriting"
            />
          </div>
          <div class="flex items-center justify-between">
            <button
              class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline"
              type="submit"
            >
              Kirish
            </button>
          </div>
        </form>
      </div>
    </transition>

    <transition>
      <div
        class="user-info-modal bg-white flex flex-col items-center justify-center gap-4 rounded-lg shadow-lg"
        v-if="succes"
      >
        <div class="icon text-green-500">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            fill="none"
            viewBox="0 0 24 24"
            stroke-width="1.5"
            stroke="currentColor"
            class="w-36 h-36"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M9 12.75L11.25 15 15 9.75M21 12a9 9 0 11-18 0 9 9 0 0118 0z"
            />
          </svg>
        </div>
        <h1 class="text-4xl font-bold text-stone-900">Tizimga kirdingiz.</h1>
      </div>
    </transition>

    <transition>
      <div class="bg w-full h-full fixed">
        <div
          class="bg-inside bg-gradient-to-r from-zinc-900 to-gray-900 w-full h-full opacity-90"
        ></div>
      </div>
    </transition>

    <div
      class="overlay"
      v-if="succes"
      @click="userInfoModal = !userInfoModal"
    ></div>

    <nav class="border-gray-700 relative z-5">
      <div class="relative flex h-24 items-center justify-between mx-auto">
        <div
          class="flex items-center justify-between w-full max-w-screen-2xl mx-auto px-4"
        >
          <div class="flex items-center gap-2">
            <img
              class="block h-16 w-auto"
              src="./assets/img/logo.png"
              alt="image"
            />
          </div>
          <div class="relative">
            <button
              @click.prevent="userInfoModal = !userInfoModal"
              class="text-white btn-user select-none flex gap-2 items-center cursor-pointer"
            >
              <svg
                xmlns="http://www.w3.org/2000/svg"
                fill="none"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="currentColor"
                class="w-8 h-8"
              >
                <path
                  stroke-linecap="round"
                  stroke-linejoin="round"
                  d="M17.982 18.725A7.488 7.488 0 0012 15.75a7.488 7.488 0 00-5.982 2.975m11.963 0a9 9 0 10-11.963 0m11.963 0A8.966 8.966 0 0112 21a8.966 8.966 0 01-5.982-2.275M15 9.75a3 3 0 11-6 0 3 3 0 016 0z"
                />
              </svg>
            </button>
            <transition>
              <div
                v-if="userInfoModal"
                class="shadow-2xl user-info absolute top-full right-0 w-72 bg-gray-50 border-gray-700 rounded-md mt-6 px-6 py-4"
              >
                <h2
                  class="text-2xl font-semibold leading-6 text-gray-950 mb-4 text-center font-extrabold"
                >
                  Foydalanuvchi
                </h2>
                <div class="group flex items-center gap-2 rounded-lg py-4">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke-width="1.5"
                    stroke="currentColor"
                    class="w-6 h-6"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M17.982 18.725A7.488 7.488 0 0012 15.75a7.488 7.488 0 00-5.982 2.975m11.963 0a9 9 0 10-11.963 0m11.963 0A8.966 8.966 0 0112 21a8.966 8.966 0 01-5.982-2.275M15 9.75a3 3 0 11-6 0 3 3 0 016 0z"
                    />
                  </svg>

                  <h2 class="text-xl text-gray-950">
                    {{ userData?.username }}
                  </h2>
                </div>
                <div class="group flex items-center gap-2 rounded-lg py-4">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke-width="1.5"
                    stroke="currentColor"
                    class="w-6 h-6"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M2.25 6.75c0 8.284 6.716 15 15 15h2.25a2.25 2.25 0 002.25-2.25v-1.372c0-.516-.351-.966-.852-1.091l-4.423-1.106c-.44-.11-.902.055-1.173.417l-.97 1.293c-.282.376-.769.542-1.21.38a12.035 12.035 0 01-7.143-7.143c-.162-.441.004-.928.38-1.21l1.293-.97c.363-.271.527-.734.417-1.173L6.963 3.102a1.125 1.125 0 00-1.091-.852H4.5A2.25 2.25 0 002.25 4.5v2.25z"
                    />
                  </svg>
                  <h2 class="text-xl text-gray-950">
                    {{ userData?.phone_number }}
                  </h2>
                </div>
              </div>
            </transition>
          </div>
        </div>
      </div>
    </nav>

    <transition>
      <div
        class="wrapper mx-auto bg-neutral-100 rounded-lg"
        v-if="dataReceived"
      >
        <h1 class="text-5xl font-semibold text-gray-800 text-center mb-4 pt-4">
          Tegishli odamlar
        </h1>
        <div class="users p-8">
          <div class="user" v-for="u in users" :key="u.id">
            <figure
              class="h-full bg-white shadow-xl rounded-md py-6 px-6"
            >
              <figcaption class="flex items-center space-x-4 text-gray-400 h-full">
                <svg
                    xmlns="http://www.w3.org/2000/svg"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke-width="1.5"
                    stroke="currentColor"
                    class="w-24 h-24"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M17.982 18.725A7.488 7.488 0 0012 15.75a7.488 7.488 0 00-5.982 2.975m11.963 0a9 9 0 10-11.963 0m11.963 0A8.966 8.966 0 0112 21a8.966 8.966 0 01-5.982-2.275M15 9.75a3 3 0 11-6 0 3 3 0 016 0z"
                    />
                  </svg>
                <div class="flex user-text flex-col justify-between">
                  <div class="text-2xl text-gray-950 font-semibold">
                    {{ u.first_name }}
                    {{ u.last_name }}
                  </div>
                  <button
                    @click.prevent="
                      (videoCall = !videoCall),
                        (calledUser = item),
                        call(u.phone_number)
                    "
                    class="call-btn w-full px-6 py-2 bg-green-600 rounded font-bold text-white mt-2"
                  >
                    Qo'ng'iroq
                  </button>
                </div>
              </figcaption>
            </figure>
          </div>
        </div>
      </div>
    </transition>

    <transition>
      <div
        class="fixed z-5 video-call rounded-lg bg-neutral-100 shadow-2xl"
        v-if="videoCall"
      >
        <div class="screen bg-azure-100 flex justify-center items-center">
          <figure
            v-if="!answerStatus"
            class="calling-screen flex flex-col-reverse rounded-md py-6 px-6"
          >
            <figcaption class="flex flex-col items-center space-x-4 gap-4">
              <img
                src="https://picsum.photos/id/45/100"
                alt=""
                class="flex-none w-44 h-44 rounded-full object-cover"
              />
              <div class="flex flex-col text-center items-center gap-6">
                <div class="text-4xl text-gray-950 font-semibold">
                  Sherali Bobakalonov {{ calledUser }}
                </div>
                <h2
                  class="inline-flex text-3xl text-gray-700 font-semibold calling"
                >
                  <span>Calling</span>
                </h2>
              </div>
            </figcaption>
          </figure>
          <video
            v-show="answerStatus"
            id="remoteVideo"
            ref="remoteVideo"
            autoplay
            muted
            playsinline
          ></video>
        </div>
        <div class="client">
          <video
            v-show="answerStatus"
            id="localVideo"
            ref="localVideo"
            autoplay
            muted
            playsinline
          ></video>
        </div>
        <div class="btn-group">
          <button
            class="btn btn-toggle shadow-xl bg-white"
            @click="(e) => e.target.classList.toggle('disabled')"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-8 h-8"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M12 18.75a6 6 0 006-6v-1.5m-6 7.5a6 6 0 01-6-6v-1.5m6 7.5v3.75m-3.75 0h7.5M12 15.75a3 3 0 01-3-3V4.5a3 3 0 116 0v8.25a3 3 0 01-3 3z"
              />
            </svg>
          </button>
          <button
            class="btn btn-end shadow-xl bg-red-600 text-white"
            @click.prevent="(videoCall = !videoCall), stop()"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-8 h-8"
            >
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M15.75 3.75L18 6m0 0l2.25 2.25M18 6l2.25-2.25M18 6l-2.25 2.25m1.5 13.5c-8.284 0-15-6.716-15-15V4.5A2.25 2.25 0 014.5 2.25h1.372c.516 0 .966.351 1.091.852l1.106 4.423c.11.44-.054.902-.417 1.173l-1.293.97a1.062 1.062 0 00-.38 1.21 12.035 12.035 0 007.143 7.143c.441.162.928-.004 1.21-.38l.97-1.293a1.125 1.125 0 011.173-.417l4.423 1.106c.5.125.852.575.852 1.091V19.5a2.25 2.25 0 01-2.25 2.25h-2.25z"
              />
            </svg>
          </button>
          <button
            class="btn btn-toggle shadow-xl bg-white"
            @click="(e) => e.target.classList.toggle('disabled')"
          >
            <svg
              xmlns="http://www.w3.org/2000/svg"
              fill="none"
              viewBox="0 0 24 24"
              stroke-width="1.5"
              stroke="currentColor"
              class="w-8 h-8"
            >
              <path
                stroke-linecap="round"
                d="M15.75 10.5l4.72-4.72a.75.75 0 011.28.53v11.38a.75.75 0 01-1.28.53l-4.72-4.72M4.5 18.75h9a2.25 2.25 0 002.25-2.25v-9a2.25 2.25 0 00-2.25-2.25h-9A2.25 2.25 0 002.25 7.5v9a2.25 2.25 0 002.25 2.25z"
              />
            </svg>
          </button>
        </div>
      </div>
    </transition>
  </div>
</template>

<script setup>
import { ref } from "vue";

import ring from './assets/audio/ringtone.mp3' 
import end from './assets/audio/ended.mp3' 

const audio = new Audio(ring);
const audioEnd = new Audio(end);


let answerStatus = ref(false);
let loginModal = ref(true);
const userInfoModal = ref(false);
const videoCall = ref(false);
const calledUser = ref(0);
let phone = ref("");
let password = ref("");
const userData = ref([]);
let succes = ref(false);
const users = ref([]);
const userToken = ref("");
const dataReceived = ref(false);


let socket;
let callSocket;

const baseURL = "/";

let localVideo = document.querySelector("#localVideo");
let remoteVideo = document.querySelector("#remoteVideo");

let otherUser;
let remoteRTCMessage;
let iceCandidatesFromCaller = [];
let peerConnection;
let remoteStream;
let localStream;
let callInProgress = false;

const getUsers = async () => {
  const res = await fetch(
    "https://cfc5-213-230-76-20.ngrok-free.app/api/accounts/related/",
    {
      method: "GET",
      headers: {
        Authorization: "Bearer " + userToken.value,
      },
    }
  );
  const data = await res.json();
  users.value = data;
  console.log(data);
  succes.value = false;
  dataReceived.value = true;
};

const login = async () => {
  const res = await fetch(
    "https://cfc5-213-230-76-20.ngrok-free.app/api/accounts/login/",
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        phone_number: phone.value,
        password: password.value,
      }),
    }
  );
  const data = await res.json();
  userData.value = data;
  if (userData.value.token) {
    succes.value = true;
    userToken.value = userData.value.token;
  }
  loginModal.value = false;
  connectSocket();
  getUsers();
};

//event from html
function call(number) {
  otherUser = number;

  beReady().then(() => {
    processCall(number);
  });
}

//event from html
function answer() {
  //do the event firing

  beReady().then((bool) => {
    processAccept();
  });

  document.getElementById("answer").style.display = "none";
}

let pcConfig = {
  iceServers: [
    {
      urls: "stun:relay.metered.ca:80",
    },
    {
      urls: "turn:relay.metered.ca:80",
      username: "18aea579f3aaeb01232f82e1",
      credential: "v3Y87dwRfn/sZURQ",
    },
    {
      urls: "turn:relay.metered.ca:443",
      username: "18aea579f3aaeb01232f82e1",
      credential: "v3Y87dwRfn/sZURQ",
    },
    {
      urls: "turn:relay.metered.ca:443?transport=tcp",
      username: "18aea579f3aaeb01232f82e1",
      credential: "v3Y87dwRfn/sZURQ",
    },
  ],
};

// Set up audio and video regardless of what devices are present.
let sdpConstraints = {
  offerToReceiveAudio: true,
  offerToReceiveVideo: true,
};

/////////////////////////////////////////////

function connectSocket() {
  let ws_scheme = window.location.protocol == "https:" ? "wss://" : "ws://";

  callSocket = new WebSocket(
    "wss://cfc5-213-230-76-20.ngrok-free.app/ws/call/"
  );
  console.log(callSocket);

  callSocket.onopen = (event) => {
    //let's send myName to the socket
    callSocket.send(
      JSON.stringify({
        type: "login",
        data: {
          name: phone.value,
        },
      })
    );
  };

  callSocket.onmessage = (e) => {
    let response = JSON.parse(e.data);

    let type = response.type;

    if (type == "connection") {
      console.log(response.data.message);
    }

    if (type == "offer_received") {
      onNewCall(response);
    }

    if (type == "call_answered") {
      answerStatus.value = true;
      audio.pause()
      onCallAnswered(response.data);
    }

    if (type == "ice_candidate") {
      onICECandidate(response.data);
    }
  };

  const onNewCall = (data) => {
    //when other called you
    //show answer button
    console.log(data);
    otherUser = data.name;
    remoteRTCMessage = { type: "offer", sdp: data.data };

    // document.getElementById("profileImageA").src = baseURL + callerProfile.image;
    document.getElementById("callerName").innerHTML = otherUser;
    document.getElementById("call").style.display = "none";
    document.getElementById("answer").style.display = "block";
  };

  const onCallAnswered = (data) => {
    
    //when other accept our call
    remoteRTCMessage = data.rtcMessage;
    peerConnection.setRemoteDescription(
      new RTCSessionDescription(remoteRTCMessage)
    );

    document.getElementById("calling").style.display = "none";

    console.log("Call Started. They Answered");

    callProgress();
  };

  const onICECandidate = (data) => {
    console.log("GOT ICE candidate");

    let message = data;

    let candidate = new RTCIceCandidate({
      sdpMLineIndex: message.sdpMLineIndex,
      candidate: message.sdpCandidate,
    });

    if (peerConnection) {
      console.log("ICE candidate Added");
      peerConnection.addIceCandidate(candidate);
    } else {
      console.log("ICE candidate Pushed");
      iceCandidatesFromCaller.push(candidate);
    }
  };
}

/**
 *
 * @param {Object} data
 * @param {number} data.name - the name of the user to call
 * @param {Object} data.rtcMessage - the rtc create offer object
 */
function sendCall(data) {
  //to send a call
  console.log("Send Call");

  // socket.emit("call", data);
  callSocket.send(
    JSON.stringify({
      type: "call",
      data,
    })
  );

  document.getElementById("call").style.display = "none";
  // document.getElementById("profileImageCA").src = baseURL + otherUserProfile.image;
  document.getElementById("otherUserNameCA").innerHTML = otherUser;
  document.getElementById("calling").style.display = "block";
}

/**
 *
 * @param {Object} data
 * @param {number} data.caller - the caller name
 * @param {Object} data.rtcMessage - answer rtc sessionDescription object
 */
function answerCall(data) {
  //to answer a call
  // socket.emit("answerCall", data);
  let target = data.caller;
  let answer_data = data.rtcMessage;

  callSocket.send(
    JSON.stringify({
      type: "create_answer",
      target: target,
      data: answer_data,
    })
  );
  callProgress();
}

/**
 *
 * @param {Object} data
 * @param {number} data.user - the other user //either callee or caller
 * @param {Object} data.rtcMessage - iceCandidate data
 */
function sendICEcandidate(data) {
  //send only if we have caller, else no need to
  console.log("Send ICE candidate");
  // socket.emit("ICEcandidate", data)
  callSocket.send(
    JSON.stringify({
      type: "ice_candidate",
      data,
    })
  );
}

function beReady() {
  // Request video stream from user's camera
  return navigator.mediaDevices
    .getUserMedia({
      audio: true,
      video: true,
    })
    .then((stream) => {
      // Save stream to global variable for later use
      localStream = stream;
      // Display video stream in localVideo element
      localVideo.srcObject = stream;
      // Create WebRTC connection and add stream to it
      return createConnectionAndAddStream();
    })
    .catch(function (e) {
      // Handle getUserMedia errors
      if (e.name === "NotAllowedError" || e.name === "NotFoundError") {
        alert("Please grant camera access to use this feature.");
      } else {
        console.log(e);
        alert("getUserMedia() error: " + e.name);
      }
    });
}

function createConnectionAndAddStream() {
  createPeerConnection();
  localStream.getTracks().forEach((track) => {
    peerConnection.addTrack(track, localStream);
  });
  peerConnection.ontrack = (event) => {
    event.streams[0].getTracks().forEach((track) => {
      remoteStream.addTrack(track);
    });
  };

  return true;
}
function processCall(userName) {
  audio.play()
  peerConnection.createOffer(
    (sessionDescription) => {
      peerConnection.setLocalDescription(sessionDescription);
      sendCall({
        name: userName,
        rtcMessage: sessionDescription,
      });
    },
    (error) => {
      console.log("Error");
    }
  );
}

function processAccept() {
  peerConnection.setRemoteDescription(
    new RTCSessionDescription(remoteRTCMessage)
  );
  peerConnection.createAnswer(
    (sessionDescription) => {
      peerConnection.setLocalDescription(sessionDescription);

      if (iceCandidatesFromCaller.length > 0) {
        //I am having issues with call not being processed in real world (internet, not local)
        //so I will push iceCandidates I received after the call arrived, push it and, once we accept
        //add it as ice candidate
        //if the offer rtc message contains all thes ICE candidates we can ingore this.
        for (let i = 0; i < iceCandidatesFromCaller.length; i++) {
          //
          let candidate = iceCandidatesFromCaller[i];
          console.log("ICE candidate Added From queue");
          try {
            peerConnection
              .addIceCandidate(candidate)
              .then((done) => {
                console.log(done);
              })
              .catch((error) => {
                console.log(error);
              });
          } catch (error) {
            console.log(error);
          }
        }
        iceCandidatesFromCaller = [];
        console.log("ICE candidate queue cleared");
      } else {
        console.log("NO Ice candidate in queue");
      }

      answerCall({
        caller: otherUser,
        rtcMessage: sessionDescription,
      });
    },
    (error) => {
      console.log("Error");
    }
  );
}

/////////////////////////////////////////////////////////

function createPeerConnection() {
  try {
    peerConnection = new RTCPeerConnection(pcConfig);
    // peerConnection = new RTCPeerConnection();
    peerConnection.onicecandidate = handleIceCandidate;
    peerConnection.onaddstream = handleRemoteStreamAdded;
    peerConnection.onremovestream = handleRemoteStreamRemoved;
    console.log("Created RTCPeerConnnection");
    return;
  } catch (e) {
    console.log("Failed to create PeerConnection, exception: " + e.message);
    alert("Cannot create RTCPeerConnection object.");
    return;
  }
}

function handleIceCandidate(event) {
  // console.log('icecandidate event: ', event);
  if (event.candidate) {
    console.log("Local ICE candidate");
    // console.log(event.candidate.candidate);

    sendICEcandidate({
      user: otherUser,
      rtcMessage: {
        sdpMLineIndex: event.candidate.sdpMLineIndex,
        sdpMid: event.candidate.sdpMid,
        sdpCandidate: event.candidate.candidate,
      },
    });
  } else {
    console.log("End of candidates.");
  }
}

function handleRemoteStreamAdded(event) {
  remoteStream = event.stream;
  console.log("Remote stream added. ", remoteStream);
  remoteVideo.srcObject = remoteStream;
}

function handleRemoteStreamRemoved(event) {
  console.log("Remote stream removed. Event: ", event);
  remoteVideo.srcObject = null;
  localVideo.srcObject = null;
}

window.onbeforeunload = function () {
  if (callInProgress) {
    stop();
  }
};

function stop() {
  audio.pause()
  audioEnd.play()
  localStream.getTracks().forEach((track) => track.stop());
  callInProgress = false;
  peerConnection.close();
  peerConnection = null;
  document.getElementById("call").style.display = "block";
  document.getElementById("answer").style.display = "none";
  document.getElementById("inCall").style.display = "none";
  document.getElementById("calling").style.display = "none";
  document.getElementById("endVideoButton").style.display = "none";
  otherUser = null;
}

function callProgress() {
  document.getElementById("videos").style.display = "block";
  document.getElementById("otherUserNameC").innerHTML = otherUser;
  document.getElementById("inCall").style.display = "block";

  callInProgress = true;
}
</script>

<style>
.bg {
  background-image: url(./assets/img/bg-image.jpg);
  background-repeat: no-repeat;
  background-size: cover;
}
</style>