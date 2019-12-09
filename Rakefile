require 'json'

WORD_CLASSES = [
  'adj.',
  'adv.',
  'conj.',
  'determiner',
  'n.',
  'past part.',
  'prep.',
  'pron.',
  'v.',
]

PARTS_OF_PHRASES = [
  '(to)',
  'care of',
  'down',
  'for',
  'go of',
  'into',
  'one',
  'opposed to',
  'out',
  'up',
  'with',
]

def get_partitions(candidates)
  0.upto(candidates.size - 1).map do |n|
    [candidates[0..n], candidates[(n + 1)..-1]]
  end.reverse
end

def parse(str)
  str.split(/\n/).map do |line|
    rest, *word_classes = line.split(/\s*,\s*/)

    word, *candidates = rest.split(/\s+/)

    if candidates.any?
      partitions = get_partitions(candidates)

      partitions.each do |(trailing, rest)|
        trailing = trailing.join(' ')

        if PARTS_OF_PHRASES.include?(trailing)
          word += " #{trailing}"
          candidates = rest
          break
        end
      end
    end

    if candidates.any?
      word_class = candidates.join(' ')

      if WORD_CLASSES.include?(word_class)
        word_classes.unshift(word_class)
      else
        raise "unknown word class: #{word_class.inspect}"
      end
    end

    { word: word, word_classes: word_classes }
  end
end

task :default => :parse

task :parse do
  entries = parse(File.read('longman-american-defining-vocabulary.txt'))
  File.write('longman-american-defining-vocabulary.json', JSON.pretty_generate(entries))
end
