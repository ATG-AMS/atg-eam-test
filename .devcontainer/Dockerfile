# Node.js LTS Bullseye 이미지를 가져옵니다.
FROM mcr.microsoft.com/devcontainers/typescript-node:1-20-bullseye

# 소스 코드의 출처를 표시합니다.
LABEL org.opencontainers.image.source="https://github.com/ATG-AMS/atg-eam-test"

# npm을 최신 안정 버전으로 업데이트하고 git을 설치합니다.
# apt-get install에서 발생할 수 있는 대화형 프롬프트를 방지하기 위해 DEBIAN_FRONTEND 환경 변수를 noninteractive로 설정합니다.
RUN npm install -g npm@latest --no-cache && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y git && \
    rm -rf /var/lib/apt/lists/*

USER node

# 스크립트 파일을 /scripts 디렉토리에 복사하고 실행 가능하게 만듭니다.
COPY --chown=node:node script.sh healthcheck.sh /scripts/
RUN chmod +x /scripts/script.sh /scripts/healthcheck.sh

# 작업 디렉토리를 설정합니다.
WORKDIR /app

# 외부에서 접근 가능한 포트를 엽니다. (5000 : 일반적 웹 애플리케이션, API 서버가 사용할 포트 / 5050 : 차후 Storybook 등을 사용하게 된다면 사용할 포트)
EXPOSE 5000
EXPOSE 5050

# HEALTHCHECK를 설정합니다.
HEALTHCHECK --interval=30s --timeout=30s --start-period=5s --retries=3 CMD ["/scripts/healthcheck.sh"]

# 컨테이너가 시작될 때 script.sh를 실행합니다.
CMD [ "sh", "/scripts/script.sh" ]
