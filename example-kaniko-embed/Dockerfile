FROM gcr.io/kaniko-project/executor@sha256:4571cdfcf91b1d1f4f7a508f6db91b14f3f0b8225cbc87e711cef79eb27cbaea as kaniko

FROM busybox@sha256:2a03a6059f21e150ae84b0973863609494aad70f0a80eaeb64bddd8d92465812

COPY --from=kaniko /kaniko/executor /kaniko/executor
