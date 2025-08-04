FROM cgr.dev/chainguard/ruby:latest-dev

# Switch to root
USER root

# Install vscode dependencies + git + curl, reload dynamic links
RUN apk add --no-cache \
  posix-libc-utils \
  libstdc++ \
  dumb-init \
  git \
  git-lfs \
  curl \
  && ldconfig


# Environment variable for system
ENV GEM_HOME=/usr/local/vendor
ENV GEM_PATH=${GEM_PATH}:/usr/local/vendor
ENV PATH=${GEM_HOME}/bin:${PATH}
ENV LANG C.UTF-8



# Update gems
RUN echo "gem: --no-ri --no-rdoc" > ~/.gemrc
RUN yes | gem update --system && gem cleanup
RUN yes | gem install jekyll bundler && gem cleanup
RUN chown -R nonroot:nonroot /usr/local/vendor

# Switch back to non-root user
USER nonroot
