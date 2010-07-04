task :initialize_working_directory do
  require 'fileutils'

  root_path = File.expand_path(File.dirname(__FILE__))

  FileUtils.mkpath(File.join(root_path, 'var'))
  FileUtils.mkpath(File.join(root_path, 'var/registry'))
  FileUtils.mkpath(File.join(root_path, 'var/run'))
  FileUtils.mkpath(File.join(root_path, 'dev'))
  FileUtils.mkpath(File.join(root_path, 'log'))
  FileUtils.mkpath(File.join(root_path, 'tmp'))

  FileUtils.touch(File.join(root_path, 'log/gammu.log'))
  FileUtils.touch(File.join(root_path, 'log/gammut.log'))
end

task :initialize_udev_rules do
  require 'fileutils'

  root_path = File.expand_path(File.dirname(__FILE__))

  # Bus 001 Device 012: ID 12d1:1001 Huawei Technologies Co., Ltd. E620 USB Modem
  UDEV_RULE_CODE = <<URC
ATTRS{idVendor}=="12d1", ATTRS{idProduct}=="1001", RUN+="#{File.join(root_path, 'bin/update_usb')}"
URC

  UDEV_RULE_DIR = '/etc/udev/rules.d'
  UDEV_RULE_FILENAME = '90-gammut.rules'

  udev_rule_path = File.join(UDEV_RULE_DIR, UDEV_RULE_FILENAME)
  File.open(udev_rule_path, 'w') do |f|
    f.write(UDEV_RULE_CODE)
  end
end

desc "Initialize local gammut tree"
task :initialize => [ :initialize_working_directory ] do
  root_path = File.expand_path(File.dirname(__FILE__))

  puts "Alrighty, initialized #{root_path}"
end

desc "Re-initialize db schema"
task :dbcreate do
  `echo 'drop database smsd;' | mysql`
  `echo 'create database smsd;' | mysql`
  `cat lib/gammu-smsd.mysql.sql | mysql smsd`
  `cat lib/gammu-smsd-extensions.mysql.sql | mysql smsd`
end
