<template>
  <v-layout wrap style="height: 200px">
    <v-container>
      <v-layout justify-center>
        <v-btn color="pink" dark @click.stop="drawer = !drawer">
          Start Video
        </v-btn>
      </v-layout>
    </v-container>
    <v-container fluid fill-height>
      <v-layout align-center justify-center>
        <v-flex>
          <v-navigation-drawer
            v-model="drawer"
            absolute
            temporary
            :width="465"
            right
          >
            <v-card align="center" justify="center">
              <v-toolbar color="primary" dark>Welcome back webrtc</v-toolbar>
              <v-card-text>
                <div class="row">
                  <div class="col-md-12">
                    <div class="">
                      <vue-webrtc
                        ref="webrtc"
                        width="100%"
                        :roomId="roomId"
                        v-on:joined-room="logEvent"
                        v-on:left-room="logEvent"
                        v-on:opened-room="logEvent"
                        v-on:share-started="logEvent"
                        v-on:share-stopped="logEvent"
                        @error="onError"
                      />
                    </div>
                    <!-- <div class="row">
                  <div class="col-md-12 my-3">
                    <v-btn elevation="2" @click="onJoin">Join</v-btn>
                    <v-btn elevation="2" @click="onLeave">Leave</v-btn>
                    <v-btn elevation="2" @click="onCapture"
                      >Capture Photo</v-btn
                    >
                    <v-btn elevation="2" @click="onShareScreen"
                      >Share Screen</v-btn
                    >
                  </div>
                </div> -->
                  </div>
                </div>
                <div class="row">
                  <div class="col-md-12" v-if="img">
                    <h2>Captured Image</h2>
                    <figure class="figure">
                      <img :src="img" class="img-responsive" />
                    </figure>
                  </div>
                </div>
              </v-card-text>
            </v-card>
          </v-navigation-drawer>
        </v-flex>
      </v-layout>
    </v-container>
  </v-layout>
</template>

<script>
export default {
  data() {
    return {
      drawer: null,
      img: null,
      roomId: 'public-room-v2',
    };
  },
  watch: {
    drawer(value) {
      if (value) this.onJoin();
      else this.onLeave();
    },
  },
  methods: {
    onCapture() {
      this.img = this.$refs.webrtc.capture();
    },
    onJoin() {
      this.$refs.webrtc.join();
    },
    onLeave() {
      this.$refs.webrtc.leave();
    },
    onShareScreen() {
      this.img = this.$refs.webrtc.shareScreen();
    },
    onError(error, stream) {
      console.log('On Error Event', error, stream);
    },
    logEvent(event) {
      console.log('Event : ', event);
    },
  },
};
</script>
