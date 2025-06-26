import streamlit as st
from streamlit_webrtc import webrtc_streamer, VideoTransformerBase
import cv2
import av
import numpy as np
import datetime

class VideoTransformer(VideoTransformerBase):
    def __init__(self):
        self.frame = None

    def transform(self, frame):
        img = frame.to_ndarray(format="bgr24")
        self.frame = img
        return img

st.title("📸 Take a Photo with Webcam")

ctx = webrtc_streamer(
    key="photo",
    video_transformer_factory=VideoTransformer,
    media_stream_constraints={"video": True, "audio": False},
    async_transform=True
)

if ctx.video_transformer:
    if st.button("📸 Capture Photo"):
        img = ctx.video_transformer.frame
        if img is not None:
            timestamp = datetime.datetime.now().strftime("%Y%m%d_%H%M%S")
            filename = f"photo_{timestamp}.jpg"
            cv2.imwrite(filename, img)
            st.success(f"Photo captured and saved as {filename}")
            st.image(img, channels="BGR", caption="Captured Image")
        else:
            st.warning("No frame available yet. Please wait...")
