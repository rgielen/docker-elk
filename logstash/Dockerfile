ARG ELK_VERSION

FROM rgielen/postfix-grok-patterns as postfix-grok-patterns

FROM rgielen/logstash-grok-patterns as logstash-grok-patterns

# https://github.com/elastic/logstash-docker
FROM docker.elastic.co/logstash/logstash:${ELK_VERSION}

# Add your logstash plugins setup here
# Example: RUN logstash-plugin install logstash-filter-json

USER root
RUN mkdir -p /etc/logstash/patterns.d
COPY --from=logstash-grok-patterns /logstash-grok-patterns/*.grok /etc/logstash/patterns.d/
COPY --from=postfix-grok-patterns /postfix-grok-patterns/*.grok /etc/logstash/patterns.d/

USER logstash
