# sumo-project

export SUMO_HOME /home/dts/Desktop/sumo-project/sumo"

sudo apt-get install git cmake python3 g++ libxerces-c-dev libfox-1.6-dev libgdal-dev libproj-dev libgl2ps-dev python3-dev swig default-jdk maven libeigen3-dev
git clone --recursive https://github.com/eclipse-sumo/sumo
cd sumo
export SUMO_HOME="$PWD"
cmake -B build .
cmake --build build -j$(nproc)

# Example

./sumo/bin/netedit odense.net.xml

./sumo/bin/netconvert --osm-files map.osm --output-file odense.net.xml --keep-edges.by-vclass passenger --remove-edges.isolated --speed-in-kmh --tls.guess-signals --tls.default-type actuated --junctions.join

./sumo/tools/randomTrips.py -n /home/dts/Desktop/sumo-project/odense.net.xml -o /home/dts/Desktop/sumo-project/odense.trips.xml -e 1000 --validate --remove-loops --allow-fringe --period 2.0 --binomial 100 --trip-attributes 'departLane="best" departSpeed="max"'

./sumo/bin/duarouter -n /home/dts/Desktop/sumo-project/odense.net.xml --route-files /home/dts/Desktop/sumo-project/odense.trips.xml -o /home/dts/Desktop/sumo-project/odense.rou.xml --ignore-errors

./sumo/tools/tlsCycleAdaptation.py -n odense.net.xml -r odense.rou.xml

./sumo/bin/sumo-gui odense.sumocfg