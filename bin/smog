#!/usr/bin/env ruby

require 'net/ssh'

DHCP_HOST = '172.23.0.1'
HOST = ARGV[0]
USER = ARGV[1]
KEY = ARGV[2]
CMD = ARGV[3]
PARA1 = ARGV[4]

def ping ip
  begin
    r = `ping -c 1 -t1 -W1 #{ip} |grep packets`
    r.match(/transmitted, (.*?) packets/)[1].to_i == 1
  rescue
    false
  end
end

def exe(ssh, cmd)
  output = ''
  ssh.exec!(cmd) do |channel, stream, data|
    output << "#{data}" if stream == :stdout
  end
  output
end

def get_ip(mac)
  Net::SSH.start(DHCP_HOST, USER, :password => KEY) do |ssh|
    exe(ssh, "cat /var/log/syslog | grep #{mac} | grep DHCPACK | tail -n1 | cut -d \" \" -f 8")
  end
end

raise "#{HOST} not reachable" unless ping(HOST)

Net::SSH.start(HOST, USER, :password => KEY, :keys => [ KEY ]) do |ssh|
  case CMD
  when 'new'
    t = PARA1.nil? ? 'ubuntu1604' : PARA1
    n = rand(100**10)
    ret = exe(ssh, "/usr/bin/vmrun -T ws clone /home/dabo/vms/_templates/#{t}/#{t}.vmx /home/dabo/vms/vmx/#{n}/#{n}.vmx linked")
    if ret.empty?
      puts "OK\t#{n}"
    else
      puts "KO\t#{ret}"
    end
  when 'list'
    ret = exe(ssh, "ls /home/dabo/vms/vmx")
    vms = ret.split
    ret2 = exe(ssh, "/usr/bin/vmrun -T ws list")
    vms_state = ret2.split("\n")
    puts "Count\t#{vms.count}"
    puts "ID\t\t\tVMX\t\t\t\t\t\t\t\t\tSTATE\tMAC\t\t\tIP"
    vms.each do |v|
      p = "/home/dabo/vms/vmx/#{v}/#{v}.vmx"
      s = vms_state.include?(p) ? 'run' : 'halt'
      mac = exe(ssh, "cat #{p} | grep \"ddress =\" | cut -d \"\\\"\" -f 2").strip.downcase
      if s == 'run'
        ip = exe(ssh, "/usr/bin/vmrun -T ws getGuestIPAddress #{p}")
        ip = ip.match(/^Error/) ? get_ip(mac) : ip
        ip = ip.match(/^Error/) ? '' : ip
      else
        ip = ''
      end
      puts "#{v}\t#{p}\t#{s}\t#{mac}\t#{ip}"
    end
  when 'rm'
    n = PARA1
    ret = exe(ssh, "/usr/bin/vmrun -T ws deleteVM /home/dabo/vms/vmx/#{n}/#{n}.vmx")
    if ret.empty?
      puts "OK\t#{n}"
    else
      puts "KO\t#{ret}"
    end
  when 'start'
    n = PARA1
    ret = exe(ssh, "/usr/bin/vmrun -T ws start /home/dabo/vms/vmx/#{n}/#{n}.vmx nogui")
    if ret.empty?
      puts "OK\t#{n}"
    else
      puts "KO\t#{ret}"
    end
  when 'halt'
    n = PARA1
    ret = exe(ssh, "/usr/bin/vmrun -T ws stop /home/dabo/vms/vmx/#{n}/#{n}.vmx soft")
    if ret.empty?
      puts "OK\t#{n}"
    else
      puts "KO\t#{ret}"
    end
  when 'stop'
    n = PARA1
    ret = exe(ssh, "/usr/bin/vmrun -T ws stop /home/dabo/vms/vmx/#{n}/#{n}.vmx hard")
    if ret.empty?
      puts "OK\t#{n}"
    else
      puts "KO\t#{ret}"
    end
  when 'reboot'
    n = PARA1
    ret = exe(ssh, "/usr/bin/vmrun -T ws reset /home/dabo/vms/vmx/#{n}/#{n}.vmx soft")
    if ret.empty?
      puts "OK\t#{n}"
    else
      puts "KO\t#{ret}"
    end
  when 'reset'
    n = PARA1
    ret = exe(ssh, "/usr/bin/vmrun -T ws reset /home/dabo/vms/vmx/#{n}/#{n}.vmx hard")
    if ret.empty?
      puts "OK\t#{n}"
    else
      puts "KO\t#{ret}"
    end
  end
end
