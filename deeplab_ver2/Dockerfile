FROM deeplab_ver2_code

WORKDIR /opt/deeplab/code
RUN for req in $(cat python/requirements.txt) pydot; do pip install $req; done

RUN rm src/caffe/util/matio_io.cpp
RUN rm src/caffe/layers/mat_read_layer.cpp
RUN rm src/caffe/layers/mat_write_layer.cpp
RUN touch include/cublas_v2.h
RUN mkdir build
WORKDIR /opt/deeplab/code/build
RUN cmake -DCPU_ONLY=1 ..
RUN make

ENV CAFFE_ROOT=/opt/deeplab/code
ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

WORKDIR /workspace
