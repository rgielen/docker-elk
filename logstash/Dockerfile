ARG ELK_VERSION

FROM rgielen/postfix-grok-patterns as postfix-grok-patterns

FROM rgielen/logstash-grok-patterns as logstash-grok-patterns

# https://github.com/elastic/logstash-docker
FROM docker.elastic.co/logstash/logstash:${ELK_VERSION}

# Add your logstash plugins setup here
# Example: RUN logstash-plugin install logstash-filter-json
ENV PIPELINE_DIR /usr/share/logstash/pipeline/

USER root
RUN mkdir -p /etc/logstash/patterns.d
COPY --from=logstash-grok-patterns /logstash-grok-patterns/*.grok /etc/logstash/patterns.d/
COPY --from=postfix-grok-patterns /postfix-grok-patterns/*.grok /etc/logstash/patterns.d/

RUN mkdir -p /usr/share/logstash/pipeline
COPY --from=logstash-grok-patterns /logstash-grok-patterns/*.conf $PIPELINE_DIR
COPY --from=postfix-grok-patterns /postfix-grok-patterns/50-filter-postfix-combined.conf $PIPELINE_DIR
COPY pipeline/* $PIPELINE_DIR

USER logstash
