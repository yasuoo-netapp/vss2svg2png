Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/xenial64"
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y cmake git g++ libxml2-dev libfreetype6-dev libwmf-dev librevenge-dev libvisio-dev inkscape
    cd /vagrant

    #
    # Build & install libemf2svg
    #
    git clone https://github.com/kakwa/libemf2svg.git
    cd libemf2svg
    mkdir build
    cd build
    cmake ..
    make
    make install
    cd ../..

    #
    # Build & install libvisio2svg
    #
    git clone https://github.com/kakwa/libvisio2svg.git
    cd libvisio2svg
    mkdir build
    cd build
    cmake ..
    make
    make install
    cd ../..
    ldconfig

    #
    # convert vss2svg
    #
    find vss -name "*.vss" -exec vss2svg-conv "--input={}" "--output=svg" ";"

    #
    # convert svg2png
    #
    mkdir png
    for f in svg/*
    do
      inkscape --export-dpi=600 --export-png=png/`basename $f`.png --file=$f
    done
  SHELL
end
