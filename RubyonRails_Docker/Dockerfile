FROM ruby:2.4.1
ENV LANG C.UTF-8

ENV APP_ROOT /app

WORKDIR $APP_ROOT

# 最低限のものをinstall
RUN apt-get update && \
    apt-get install -y nodejs sqlite3 --no-install-recommends && \
    rm -rf /var/lib/apt/lists/*

# image内にbundle installするためGemfileをコピーする
COPY Gemfile $WORKDIR
COPY Gemfile.lock $WORKDIR
ADD . /app

# docker image内にbundle install
RUN \
  echo 'gem: --no-document' > ~/.gemrc && \
  cp ~/.gemrc /etc/gemrc && \
  chmod uog+r /etc/gemrc && \
  bundle config --global build.nokogiri --use-system-libraries && \
  bundle config --global jobs 4 && \
  bundle install && \
  rm -rf ~/.gem

# ポートのエクスポート
EXPOSE  3000

CMD ["rails", "server", "-b", "0.0.0.0"]

## ----- memo -----
# bundle config --global jobs 4でbundle install 高速化
