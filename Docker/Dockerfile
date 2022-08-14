FROM debian:buster-slim

# Prepare gtc2vcf prerequisites
RUN apt-get update && apt-get install --yes --no-install-recommends git wget gcc libc6-dev autoconf make ca-certificates zlib1g-dev libbz2-dev libssl-dev liblzma-dev libgsl-dev && rm -rf /var/lib/apt/lists/* # buildkit
RUN git clone --branch=develop --recurse-submodules https://github.com/samtools/htslib.git && git clone --branch=develop https://github.com/samtools/bcftools.git # buildkit
RUN rm -f bcftools/plugins/gtc2vcf.c bcftools/plugins/gtc2vcf.h bcftools/plugins/affy2vcf.c # buildkit
ADD . gtc2vcf
RUN cp gtc2vcf/gtc2vcf.c gtc2vcf/gtc2vcf.h gtc2vcf/affy2vcf.c bcftools/plugins # buildkit
RUN cd /htslib && autoheader && (autoconf || autoconf) && ./configure && make && cp bgzip tabix htsfile /usr/bin && cd /bcftools && make && cp bcftools /usr/bin # buildkit
ENV BCFTOOLS_PLUGINS=/bcftools/plugins

CMD ["bcftools -g /tmp/R06C01 -f /tmp/human_g1k_v37.fasta -b /tmp/GDACS_20050209_4B.bpm -e /tmp/GDA_egt_file_17-01-22.egt -c /tmp/GDA-CS_20050209_4B.gz -o /tmp/result.vcf"]