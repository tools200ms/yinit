FROM 200ms/alpinenet_dev2

#ENV PKGS_MPY="micropython"
ENV PKGS_DEP_MPY="gcc git make musl-dev libbsd-dev libffi-dev python3"

# Set `PKGS_ADD` so `200ms/alpinenet_dev2` installs these packages at build time:
ENV PKGS_ADD="$PKGS_DEP_MPY"

# Install MicroPython from sources:
ENV MPY_GIT_URL="https://github.com/micropython/micropython"
ENV MPY_SRC="/usr/src/micropython"
# wget -qO- $MPY_GIT_URL/archive/refs/heads/master.tar.gz | tar -xz --strip-components=1 -C $MPY_SRC
ENV MPY_MAKE_FLAGS="MICROPY_PY_BTREE=0"
#ENV MPY_VARIANT="VARIANT=minimal"

# Install dependenties
RUN apk add $PKGS_ADD

COPY micropython-sigterm.patch /tmp

# Compile Micropython
RUN mkdir -p $MPY_SRC && git clone $MPY_GIT_URL $MPY_SRC && git -C $MPY_SRC apply /tmp/micropython-sigterm.patch && \
    make -C $MPY_SRC/mpy-cross && cp $MPY_SRC/mpy-cross/build/mpy-cross /usr/local/bin/ && \
    make -C $MPY_SRC/ports/unix submodules && \
    make install $MPY_MAKE_FLAGS -C $MPY_SRC/ports/unix

RUN micropython -m mip install logging
RUN micropython -m mip install pprint


