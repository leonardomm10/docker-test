FROM elasticsearch:2.3.4

COPY elasticsearch.yml /usr/share/elasticsearch/config/elasticsearch.yml

RUN wget http://arcafiles.arcasolutions.com/infra/recurring-plugin-1.0.zip && /usr/share/elasticsearch/bin/plugin install file:recurring-plugin-1.0.zip

RUN echo 'ctx._source.views+=1' > /usr/share/elasticsearch/config/scripts/updateView.groovy
RUN echo "(doc['geoLocation'].empty || (doc['geoLocation'].lat == 0 && doc['geoLocation'].lon == 0) ) ? ( _source + [ distance: 0 ] ) : ( _source + [ distance: doc['geoLocation'].distanceInKm(lat.toFloat(),lon.toFloat()) ] )" > /usr/share/elasticsearch/config/scripts/searchDistance.groovy