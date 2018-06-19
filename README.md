# P2PLite
Create p2p connections using webrtc and a signalhub

## Install signal server ([signalhub](https://github.com/mafintosh/signalhub) or analog) 

```
npm install -g signalhub
```
```
npm install signalhub --save
```

## Install P2PLite

```
npm install p2plite --save
```

## Usage

``` js

import signalhub from 'signalhub'
import { P2PLite } from 'p2plite'

let user = {
  id: 200,
  username: 'Test user'
};

let server = signalhub('room-1', [
  'http://localhost:3000'
]);

navigator.mediaDevices
  .getUserMedia({
    video: true,
    audio: true
  })
  .then(stream => {
    let p2p = new P2PLite(server, stream, {
      params: {
        user: user
      }
    });

    const streams = [];

    p2p.onStream(peer => {
      streams.push(peer);

      let video = document.querySelector('video');

      if (video) {
        video.srcObject = peer.getStream();
        video.play();
      }
    });

    p2p.onSignal(peer => {
      if (peer.getId() !== p2p.getUser().getId()) {
        peer.call(true); // Create offer or call your friend
      }
    });

    this.p2p.onClose(uuid => {
      streams.forEach((item, key) => {
        if (item.getId() === uuid) {
          streams.splice(key, 1);
        }
      });
    });

    setInterval(_ => {
      if (streams.length > 0) {
        console.log('Streams', streams.length, streams);
      }
    }, 3000);
  })
  .catch(err => console.error(err.message));

```

## Demo

## API

## License

MIT
