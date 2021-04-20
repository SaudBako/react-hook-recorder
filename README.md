# react-hook-recorder

A simple react hook using [MediaRecorder API](https://developer.mozilla.org/en-US/docs/Web/API/MediaRecorder/MediaRecorder)

## Demo

https://codesandbox.io/s/react-hook-recorder-gbz6z

## Example

```javascript
import { useState, useCallback } from "react";
import useRecorder from "react-hook-recorder";

function Recorder() {
  const [url, setUrl] = useState("");
  const onStop = useCallback((blob, blobUrl) => {
    setUrl(blobUrl);
  }, []);

  const {
    startRecording,
    stopRecording,
    register,
    recording,
    ready,
  } = useRecorder();

  return (
    <div>
      <video ref={register} autoPlay muted>
        VideoRTC
      </video>
      {url && <video controls src={url} />}
      {!!ready && (
        <>
          <button onClick={startRecording} disabled={recording}>
            start
          </button>
          <button onClick={stopRecording(onStop)} disabled={!recording}>
            stop
          </button>
        </>
      )}

      <div>{recording ? "Recording" : "Standby"}</div>
    </div>
  );
}

export default Recorder;
```

## API

### _`useRecorder`_

#### `Args` (mediaStreamConstraints: MediaStreamConstraints, mediaRecorderOptions: MediaRecorderOptions)

| Property               | Required | Type     | Description                                                                                                |
| ---------------------- | -------- | -------- | ---------------------------------------------------------------------------------------------------------- |
| mediaStreamConstraints | false    | `object` | [`MediaStreamConstraints`](https://developer.mozilla.org/en-US/docs/Web/API/MediaStreamConstraints) object |
| mediaRecorderOptions   | false    | `object` | [`MediaRecorder`](https://developer.mozilla.org/en-US/docs/Web/API/MediaRecorder/MediaRecorder) object     |

#### `Returns` (object)

| Property       | Type             | Args                                        | Description                        |
| -------------- | ---------------- | ------------------------------------------- | ---------------------------------- |
| mediaRecorder  | `MediaRecorder`  |                                             | MediaRecorder instance ref         |
| stream         | `MediaStream`    |                                             | MediaStream instance ref           |
| startRecording | `function`       |                                             | function to start recording        |
| stopRecording  | `function`       | `function(blob: Blob, url: string) => void` | function to stop recording         |
| register       | `function`       | `HTMLVideoElement`                          | function to register video element |
| status         | `RecorderStatus` |                                             | return recorder status             |
| error          | `RecorderError`  |                                             | return recorder error              |

## Types

#### _`enum RecorderStatus`_ : _`"idle" | "init" | "recording"`_

#### _`enum RecorderError`_ : _`"stream-init" | "recorder-init"`_
