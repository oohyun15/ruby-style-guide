---
layout: base
title: 루비 스타일 가이드
permalink: '/'
---

# 루비 스타일 가이드

Ruby는 Shopify에서 사용하는 메인 언어입니다. 저희 소스코드는 주로 Ruby로 이뤄져 있으며 아마 Ruby로 짜여진
큰 회사들 중 하나일 것입니다. 또한 Ruby는 저희가 새로운 웹 프로젝트 및 스크립트 작성 시 즐겨 사용하는 언어입니다.

저희는 Shopify의 모든 개발자들이 최소한 Ruby에 대한 기본적인 이해는 있다고 기대합니다. 정말 대단한 언어에요. 이 언어는
여러분이 매일 어떤 일을 하든 더 나은 개발자로 만들어 줄겁니다. 아래 내용들은 Ruby로 개발하면서 따라야 할 느슨한
코딩 스타일입니다.

이 스타일 가이드는 10여 년간 Shopify에서의 Ruby 개발에 대한 결과물입니다. 가이드 내용의 대부분은 Bozhidar Batsov의
[루비 스타일 가이드](https://github.com/rubocop-hq/ruby-style-guide)에 기반을 두고 있고, Shopify의
[많은 컨트리뷰터들](https://github.com/Shopify/ruby-style-guide/graphs/contributors)에 의해 각색되어 있습니다.

### RuboCop에 적용하기

저희는 여러분의 Ruby 프로젝트들에서 이 스타일 가이드를 적용할 수 있도록 [RuboCop](https://github.com/rubocop-hq/rubocop)을
사용하는 걸 추천합니다. RuboCop 설치 및 사용 방법은 [RuboCop 공식 문서](https://docs.rubocop.org/rubocop/)를 참고해주세요.

저희는 이 스타일 가이드로 적용하고 싱크할 수 있는 기본 RuboCop 구성을 제공합니다.
이를 사용하려면 `Gemfile`에 아래 코드를 추가해 주세요.

  ~~~ruby
  gem "rubocop-shopify", require: false
  ~~~

그리고 프로젝트의 RuboCop 설정 파일의 최상단에 아래 코드를 추가해 주세요.

  ~~~yml
  inherit_gem:
    rubocop-shopify: rubocop.yml
  ~~~

그럼 이 스타일 가이드의 `Include`, `Exclude` 설정값이 RuboCop의 기본 구성에 적용됩니다.

설정 적용에 대한 더 많은 정보는 [RuboCop 문서](https://docs.rubocop.org/rubocop/configuration.html#inheriting-configuration-from-a-dependency-gem)를 참고해 주세요.

## 목차

* [일반(General)](#일반general)
* [레이아웃(Layout)](#레이아웃layout)
* [Syntax](#syntax)
* [Naming](#naming)
* [Classes and Modules](#classes-and-modules)
* [Exceptions](#exceptions)
* [Collections](#collections)
* [Strings](#strings)
* [Regular Expressions](#regular-expressions)
* [Percent Literals](#percent-literals)
* [Testing](#testing)

## 일반(General)

* 메서드의 모든 줄이 같은 추상화 단계에서 동작하게 하세요.
  (Single Level of Abstraction 원리)

* 함수형 방식으로 코딩하세요. 가능한 한 뮤테이션 (사이드 이펙트)를 피하세요.

* [방어적 프로그래밍을 피하세요.](https://web.archive.org/web/20211013164839/http://www.erlang.se/doc/programming_rules.shtml#HDR11)
  > 과한 방어적 프로그래밍은 전혀 발생하지 않을 에러를 보호해 불필요한 런타임 및 유지 보수 비용을 발생시킬 수 있습니다.

* 함수 내에서 인자값의 변화를 피하세요.

* 몽키패치 방식을 피하세요.

* 긴 메서드가 되지 않게 피하세요.

* 많은 매개변수를 갖지 않게 피하세요.

* 필요없는 메타 프로그래밍을 피하세요.

* `private`/`protected`가 우회되지 않게 `send` 보다 `public_send`를 사용하는 걸 선호하세요.

* `ruby -w`를 통해 안전한 코드를 작성하세요.

* 3개보다 많은 블록 중첩이 생기지 않게 피하세요.

## 레이아웃(Layout)

* 소스 파일 인코딩은 `UTF-8`로 사용하세요.

* 들여쓰기 시 탭이 아닌 공백 2칸을 사용하세요.

* Unix 스타일의 줄바꿈을 사용하세요.

* 명령문 및 표현식들을 구분하기 위해 `;`을 사용하는 걸 피하세요. 1줄마다 1개의 표현식을 쓰세요.

* 연산자 앞뒤, `,`, `:`, `;` 뒤, `{` 앞뒤, `}` 앞에 공백을 사용하세요.

* `(`, `[` 이후나 `]`, `)` 이전의 공백 사용을 피하세요.

* `!` 연산자 이후에 공백 사용을 피하세요.

* 범위식(e.g. `1..5`)에서는 공백 사용을 피하세요.

* 메서드 콜 앞뒤에는 공백 사용을 피하세요.

  ~~~ruby
  # 나쁜 예
  foo . bar

  # 좋은 예
  foo.bar
  ~~~

* 람다식 내에서는 공백 사용을 피하세요.

  ~~~ruby
  # 나쁜 예
  a = -> (x, y) { x + y }

  # 좋은 예
  a = ->(x, y) { x + y }
  ~~~

* `when`은 `case`와 같은 깊이로 들여쓰기를 하세요.

* 조건문을 이용해 리턴값으로 변수 할당 시, 조건문 브랜치(`if`, `else`, ...)를 변수에 맞추세요.

  ~~~ ruby
  # 나쁜 예
  result =
    if some_cond
      # ...
      # ...
      calc_something
    else
      calc_something_else
    end

  # 좋은 예
  result = if some_cond
    # ...
    # ...
    calc_something
  else
    calc_something_else
  end
  ~~~

* begin 블록 결과값으로 변수 할당 시, rescue/ensure/end를 시작 줄에 맞추세요.

  ~~~ ruby
  # 나쁜 예
  host = begin
           URI.parse(value).host
         rescue URI::Error
           nil
         end

  # 좋은 예
  host = begin
    URI.parse(value).host
  rescue URI::Error
    nil
  end
  ~~~

* 메서드 정의 사이마다, 그리고 메서드 내부적으로 논리적 단락마다 빈 줄을 사용하세요.

* 메서드 파라미터에 기본값을 할당할 때 `=` 연산자 주변에 공백을 사용하세요.

* 불필요한 `\` 줄 바꿈을 피하세요.

* 만약 메서드 호출 시 파라미터로 인해 1줄이 넘어갈 때, 파라미터를 메서드 호출 줄보다 1탭 더 들여쓰세요.

  ~~~ ruby
  # 시작 상태 (줄이 너무 김)
  def send_mail(source)
    Mailer.deliver(to: "bob@example.com", from: "us@example.com", subject: "Important message", body: source.text)
  end

  # 나쁜 예 (2번 들여쓰기)
  def send_mail(source)
    Mailer.deliver(
        to: "bob@example.com",
        from: "us@example.com",
        subject: "Important message",
        body: source.text)
  end

  # 좋은 예
  def send_mail(source)
    Mailer.deliver(
      to: "bob@example.com",
      from: "us@example.com",
      subject: "Important message",
      body: source.text,
    )
  end
  ~~~

* 여러 줄에 걸쳐 메서드 체인을 할 때, 1탭 더 들여써서 메서드를 호출하세요.

  ~~~ ruby
  # 나쁜 예 (이전 메서드 호출에 맞게 들여쓰기)
  User.pluck(:name)
      .sort(&:casecmp)
      .chunk { |n| n[0] }

  # 좋은 예
  User
    .pluck(:name)
    .sort(&:casecmp)
    .chunk { |n| n[0] }
  ~~~

* 여러 줄에 걸친 배열 원소를 정렬하세요.

* 한 줄에 최대 120자로 제한하세요.

* 줄 마지막에 공백을 피하세요.

* 정렬 목적을 제외한 여분의 공백을 피하세요.

* 모든 파일을 줄바꿈으로 끝내세요.

* 블록 주석을 피하세요.

  ~~~ ruby
  # 나쁜 예
  =begin
  comment line
  another comment line
  =end

  # 좋은 예
  # comment line
  # another comment line
  ~~~

* 메소드 호출 시 여는 괄호가 첫 번째 인자와 다른 줄에 있을 때, 닫는 괄호를 마지막 인자 뒷 줄에 사용하세요.

  ~~~ ruby
  # 나쁜 예
  method(
    arg_1,
    arg_2)

  # 좋은 예
  method(
    arg_1,
    arg_2,
  )
  ~~~

* 매직 코멘트와 코드 및 설명 주석을 빈 줄로 분리하세요.

  ~~~ruby
  # 좋은 예
  # frozen_string_literal: true

  # Some documentation for Person
  class Person
    # Some code
  end

  # 나쁜 예
  # frozen_string_literal: true
  # Some documentation for Person
  class Person
    # Some code
  end
  ~~~

* 접근 제어자(attribute accessor) 주변에 빈 줄을 사용하세요.

  ~~~ruby
  # 나쁜 예
  class Foo
    attr_reader :foo
    def foo
      # do something...
    end
  end

  # 좋은 예
  class Foo
    attr_reader :foo

    def foo
      # do something...
    end
  end
  ~~~

* 메서드, 클래스, 모듈, 블록 간에 빈 줄을 피하세요.

  ~~~ruby
  # 나쁜 예
  class Foo

    def foo

      begin

        do_something do

          something

        end

      rescue

        something

      end

      true

    end

  end

  # 좋은 예
  class Foo
    def foo
      begin
        do_something do
          something
        end
      rescue
        something
      end
    end
  end
  ~~~

## Syntax

* Use `::` only to reference constants (this includes classes and modules) and
  constructors (like `Array()` or `Nokogiri::HTML()`). Avoid `::` for
  regular method invocation.

* Avoid using `::` for defining class and modules, or for inheritance, since
  constant lookup will not search in parent classes/modules.

  ~~~ ruby
  # bad
  module A
    FOO = "test"
  end

  class A::B
    puts FOO  # this will raise a NameError exception
  end

  # good
  module A
    FOO = "test"

    class B
      puts FOO
    end
  end
  ~~~

* Use def with parentheses when there are parameters. Omit the parentheses when
  the method doesn't accept any parameters.

* Avoid `for`.

* Avoid `then`.

* Favour the ternary operator(`?:`) over `if/then/else/end` constructs.

  ~~~ ruby
  # bad
  result = if some_condition then something else something_else end

  # good
  result = some_condition ? something : something_else
  ~~~

* Use one expression per branch in a ternary operator. This also means that
  ternary operators must not be nested. Prefer if/else constructs in these
  cases.

* Avoid multiline `?:` (the ternary operator); use `if/unless` instead.

* Use `when x then ...` for one-line cases.

* Use `!` instead of `not`.

* Prefer `&&`/`||` over `and`/`or`.

* Favour `unless` over `if` for negative conditions.

* Avoid `unless` with `else`. Rewrite these with the positive case first.

* Use parentheses around the arguments of method invocations. Omit parentheses
  when not providing arguments. Also omit parentheses when the invocation is
  single-line and the method:
  - is a class method call with implicit receiver.
  - is called by syntactic sugar (e.g: `1 + 1` calls the `+` method, `foo[bar]`
    calls the `[]` method, etc).

  ~~~ ruby
  # bad
  class User
    include(Bar)
    has_many(:posts)
  end

  # good
  class User
    include Bar
    has_many :posts
    SomeClass.some_method(:foo)
  end
  ~~~

  - is one of the following methods:
    * `require`
    * `require_relative`
    * `require_dependency`
    * `yield`
    * `raise`
    * `puts`

* Omit the outer braces around an implicit options hash.

* Use the proc invocation shorthand when the invoked method is the only
  operation of a block.

  ~~~ ruby
  # bad
  names.map { |name| name.upcase }

  # good
  names.map(&:upcase)
  ~~~

* Prefer `{...}` over `do...end` for single-line blocks.

* Prefer `do..end` over `{...}` for multi-line blocks.

* Omit `return` where possible.

* Omit `self` where possible.

  ~~~ ruby
  # bad
  self.my_method

  # good
  my_method

  # also good
  attr_writer :name

  def my_method
    self.name = "Rafael" # `self` is needed to reference the attribute writer.
  end
  ~~~

* Wrap assignment in parentheses when using its return value in a conditional
  statement.

  ~~~ ruby
  if (value = /foo/.match(string))
  ~~~

* Use `||=` to initialize variables only if they're not already initialized.

* Avoid using `||=` to initialize boolean variables.

  ~~~ ruby
  # bad - would set enabled to true even if it was false
  @enabled ||= true

  # good
  @enabled = true if @enabled.nil?

  # also valid - defined? workaround
  @enabled = true unless defined?(@enabled)
  ~~~

* Avoid spaces between a method name and the opening parenthesis.

* Prefer the lambda literal syntax over `lambda`.

  ~~~ ruby
  # bad
  l = lambda { |a, b| a + b }
  l.call(1, 2)

  l = lambda do |a, b|
    tmp = a * 7
    tmp * b / 50
  end

  # good
  l = ->(a, b) { a + b }
  l.call(1, 2)

  l = ->(a, b) do
    tmp = a * 7
    tmp * b / 50
  end
  ~~~

* Prefer `proc` over `Proc.new`.

* Prefix unused block parameters with `_`. It's also acceptable to use just `_`.

* Prefer a guard clause when you can assert invalid data. A guard clause is a
  conditional statement at the top of a function that bails out as soon as it
  can.

  ~~~ ruby
  # bad
  def compute_thing(thing)
    if thing[:foo]
      update_with_bar(thing)
      if thing[:foo][:bar]
        partial_compute(thing)
      else
        re_compute(thing)
      end
    end
  end

  # good
  def compute_thing(thing)
    return unless thing[:foo]
    update_with_bar(thing[:foo])
    return re_compute(thing) unless thing[:foo][:bar]
    partial_compute(thing)
  end
  ~~~

* Prefer keyword arguments over options hash.

* Prefer `map` over `collect`, `find` over `detect`, `select` over `find_all`,
  `size` over `length`.

* Prefer `Time` over `DateTime`.

* Prefer `Time.iso8601(foo)` instead of `Time.parse(foo)` when expecting ISO8601
  formatted time strings like `"2018-03-20T11:16:39-04:00"`.

## Naming

* Use `snake_case` for symbols, methods, and variables.

* Use `CamelCase` for classes and modules, but keep acronyms like HTTP, RFC, XML
  uppercase.

* Use `snake_case` for naming files and directories, e.g. `hello_world.rb`.

* Define a single class or module per source file. Name the file name as the
  class or module, but replacing `CamelCase` with `snake_case`.

* Use `SCREAMING_SNAKE_CASE` for other constants.

* When using inject with short blocks, name the arguments according to what is
  being injected, e.g. `|hash, e|` (mnemonic: hash, element)

* When defining binary operators, name the parameter `other`(`<<` and `[]` are
  exceptions to the rule, since their semantics are different).

* Name predicate methods with a `?`. Predicate methods are methods that return a
  boolean value.

* Avoid ending method names with a `?` if they don't return a boolean.

* Avoid prefixing method names with `is_`.

  ~~~ruby
  # bad
  def is_empty?
  end

  # good
  def empty?
  end
  ~~~

* Avoid starting method names with `get_`.

* Avoid ending method names with `!` when there is no equivalent method without
  the bang. Bangs are to mark a more dangerous version of a method, e.g. `save`
  returns a boolean in ActiveRecord, whereas `save!` will throw an exception on
  failure.

* Avoid magic numbers. Use a constant and give it a meaningful name.

* Avoid nomenclature that has (or could be interpreted to have) discriminatory
  origins.

## Comments

* Include relevant context in comments, as readers might be missing it.

* Keep comments in sync with code.

* Write comments using proper capitalization and punctuation.

* Avoid superfluous comments. Focus on **why** the code is the way it is if
  this is not obvious, not **how** the code works.

## Classes and Modules

* Prefer modules to classes with only class methods. Classes should be used only
  when it makes sense to create instances out of them.

* Prefer `extend self` over `module_function`.

  ~~~ ruby
  # bad
  module SomeModule
    module_function

    def some_method
    end

    def some_other_method
    end
  end

  # good
  module SomeModule
    extend self

    def some_method
    end

    def some_other_method
    end
  end
  ~~~

* Use a `class << self` block over `def self.` when defining class methods, and
  group them together within a single block.

  ~~~ ruby
  # bad
  class SomeClass
    def self.method1
    end

    def method2
    end

    private

    def method3
    end

    def self.method4 # this is actually not private
    end
  end

  # good
  class SomeClass
    class << self
      def method1
      end

      private

      def method4
      end
    end

    def method2
    end

    private

    def method3
    end
  end
  ~~~

* Respect the [Liskov Substitution Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle)
  when designing class hierarchies.

* Use `attr_accessor`, `attr_reader`, and `attr_writer` to define trivial
  accessors and mutators.

  ~~~ ruby
  # bad
  class Person
    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end

    def first_name
      @first_name
    end

    def last_name
      @last_name
    end
  end

  # good
  class Person
    attr_reader :first_name, :last_name

    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end
  end
  ~~~

* Prefer `attr_reader` and `attr_accessor` over `attr`.

* Avoid class (`@@`) variables.

* Indent the `public`, `protected`, and `private` methods as much as the method
  definitions they apply to. Leave one blank line above the visibility modifier
  and one blank line below it.

  ~~~ ruby
  class SomeClass
    def public_method
      # ...
    end

    private

    def private_method
      # ...
    end

    def another_private_method
      # ...
    end
  end
  ~~~

* Prefer `alias_method` over `alias`.

## Exceptions

* Signal exceptions using the `raise` method.

* Omit `RuntimeError` in the two argument version of `raise`.

  ~~~ ruby
  # bad
  raise RuntimeError, "message"

  # good - signals a RuntimeError by default
  raise "message"
  ~~~

* Prefer supplying an exception class and a message as two separate arguments to
  `raise` instead of an exception instance.

  ~~~ ruby
  # bad
  raise SomeException.new("message")
  # Note that there is no way to do `raise SomeException.new("message"), backtrace`.

  # good
  raise SomeException, "message"
  # Consistent with `raise SomeException, "message", backtrace`.
  ~~~

* Avoid returning from an `ensure` block. If you explicitly return from a method
  inside an `ensure` block, the return will take precedence over any exception
  being raised, and the method will return as if no exception had been raised at
  all. In effect, the exception will be silently thrown away.

  ~~~ ruby
  # bad
  def foo
    raise
  ensure
    return "very bad idea"
  end
  ~~~

* Use implicit begin blocks where possible.

  ~~~ ruby
  # bad
  def foo
    begin
      # main logic goes here
    rescue
      # failure handling goes here
    end
  end

  # good
  def foo
    # main logic goes here
  rescue
    # failure handling goes here
  end
  ~~~

* Avoid empty `rescue` statements.

  ~~~ ruby
  # bad
  begin
    # an exception occurs here
  rescue SomeError
    # the rescue clause does absolutely nothing
  end

  # bad - `rescue nil` swallows all errors, including syntax errors, and
  # makes them hard to track down.
  do_something rescue nil
  ~~~

* Avoid `rescue` in its modifier form.

  ~~~ ruby
  # bad - this catches exceptions of StandardError class and its descendant
  # classes.
  read_file rescue handle_error($!)

  # good - this catches only the exceptions of Errno::ENOENT class and its
  # descendant classes.
  def foo
    read_file
  rescue Errno::ENOENT => error
    handle_error(error)
  end
  ~~~

* Avoid rescuing the `Exception` class.

  ~~~ ruby
  # bad
  begin
    # calls to exit and kill signals will be caught (except kill -9)
    exit
  rescue Exception
    puts "you didn't really want to exit, right?"
    # exception handling
  end

  # good
  begin
    # a blind rescue rescues from StandardError, not Exception.
  rescue => error
    # exception handling
  end
  ~~~

* Prefer exceptions from the standard library over introducing new exception
  classes.

* Use meaningful names for exception variables.

  ~~~ ruby
  # bad
  begin
    # an exception occurs here
  rescue => e
    # exception handling
  end

  # good
  begin
    # an exception occurs here
  rescue => error
    # exception handling
  end
  ~~~

## Collections

* Use literal array and hash creation notation unless you need to pass
  parameters to their constructors.

  ~~~ ruby
  # bad
  arr = Array.new
  hash = Hash.new

  # good
  arr = []
  hash = {}
  ~~~

* Prefer the literal array syntax over `%w` or `%i`.

  ~~~ ruby
  # bad
  STATES = %w(draft open closed)

  # good
  STATES = ["draft", "open", "closed"]
  ~~~

* Append a trailing comma in multi-line collection literals.

  ~~~ ruby
  # bad
  {
    foo: :bar,
    baz: :toto
  }

  # good
  {
    foo: :bar,
    baz: :toto,
  }
  ~~~

* When accessing the first or last element from an array, prefer `first` or
  `last` over `[0]` or `[-1]`.

* Avoid mutable objects as hash keys.

* Use shorthand hash literal syntax when all keys are symbols.

  ~~~ ruby
  # bad
  { :a => 1, :b => 2 }

  # good
  { a: 1, b: 2 }
  ~~~

* Prefer hash rockets syntax over shorthand syntax when not all keys are
  symbols.

  ~~~ ruby
  # bad
  { a: 1, "b" => 2 }

  # good
  { :a => 1, "b" => 2 }
  ~~~

* Prefer `Hash#key?` over `Hash#has_key?`.

* Prefer `Hash#value?` over `Hash#has_value?`.

* Use `Hash#fetch` when dealing with hash keys that should be present.

  ~~~ ruby
  heroes = { batman: "Bruce Wayne", superman: "Clark Kent" }
  # bad - if we make a mistake we might not spot it right away
  heroes[:batman] # => "Bruce Wayne"
  heroes[:supermann] # => nil

  # good - fetch raises a KeyError making the problem obvious
  heroes.fetch(:supermann)
  ~~~

* Introduce default values for hash keys via `Hash#fetch` as opposed to using
  custom logic.

  ~~~ ruby
  batman = { name: "Bruce Wayne", is_evil: false }

  # bad - if we just use || operator with falsy value we won't get the expected result
  batman[:is_evil] || true # => true

  # good - fetch work correctly with falsy values
  batman.fetch(:is_evil, true) # => false
  ~~~

* Place `]` and `}` on the line after the last element when opening
  brace is on a separate line from the first element.

  ~~~ ruby
  # bad
  [
    1,
    2]

  {
    a: 1,
    b: 2}

  # good
  [
    1,
    2,
  ]

  {
    a: 1,
    b: 2,
  }
  ~~~

## Strings

* Prefer string interpolation and string formatting instead of string
  concatenation:

  ~~~ ruby
  # bad
  email_with_name = user.name + " <" + user.email + ">"

  # good
  email_with_name = "#{user.name} <#{user.email}>"

  # good
  email_with_name = format("%s <%s>", user.name, user.email)
  ~~~

* Avoid padded-spacing inside braces in interpolated expressions.

  ~~~ ruby
  # bad
  "From: #{ user.first_name }, #{ user.last_name }"

  # good
  "From: #{user.first_name}, #{user.last_name}"
  ~~~

* Use double-quoted strings.

  ~~~ ruby
  # bad
  'Just some text'
  'No special chars or interpolation'

  # good
  "Just some text"
  "No special chars or interpolation"
  "Every string in #{project} uses double_quotes"
  ~~~

* Avoid the character literal syntax `?x`.

* Use `{}` around instance and global variables being interpolated into a
  string.

  ~~~ ruby
  class Person
    attr_reader :first_name, :last_name

    def initialize(first_name, last_name)
      @first_name = first_name
      @last_name = last_name
    end

    # bad - valid, but awkward
    def to_s
      "#@first_name #@last_name"
    end

    # good
    def to_s
      "#{@first_name} #{@last_name}"
    end
  end

  $global = 0
  # bad
  puts "$global = #$global"

  # fine, but don't use globals
  puts "$global = #{$global}"
  ~~~

* Avoid `Object#to_s` on interpolated objects.

  ~~~ ruby
  # bad
  message = "This is the #{result.to_s}."

  # good - `result.to_s` is called implicitly.
  message = "This is the #{result}."
  ~~~

* Avoid `String#gsub` in scenarios in which you can use a faster more
  specialized alternative.

  ~~~ ruby
  url = "http://example.com"
  str = "lisp-case-rules"

  # bad
  url.gsub("http://", "https://")
  str.gsub("-", "_")
  str.gsub(/[aeiou]/, "")

  # good
  url.sub("http://", "https://")
  str.tr("-", "_")
  str.delete("aeiou")
  ~~~

* When using heredocs for multi-line strings keep in mind the fact that they
  preserve leading whitespace. It's a good practice to employ some margin based
  on which to trim the excessive whitespace.

  ~~~ ruby
  code = <<-END.gsub(/^\s+\|/, "")
    |def test
    |  some_method
    |  other_method
    |end
  END
  # => "def test\n  some_method\n  other_method\nend\n"

  # In Rails you can use `#strip_heredoc` to achieve the same result
  code = <<-END.strip_heredoc
    def test
      some_method
      other_method
    end
  END
  # => "def test\n  some_method\n  other_method\nend\n"
  ~~~

* In Ruby 2.3, prefer ["squiggly
  heredoc"](https://github.com/ruby/ruby/pull/878) syntax, which has the same
  semantics as `strip_heredoc` from Rails:

  ~~~ruby
  code = <<~END
    def test
      some_method
      other_method
    end
  END
  # => "def test\n  some_method\n  other_method\nend\n"
  ~~~

* Indent heredoc contents and closing according to its opening.

  ~~~ruby
  # bad
  class Foo
    def bar
      <<~SQL
        'Hi'
    SQL
    end
  end

  # good
  class Foo
    def bar
      <<~SQL
        'Hi'
      SQL
    end
  end

  # bad

  # heredoc contents is before closing heredoc.
  foo arg,
      <<~EOS
    Hi
      EOS

  # good
  foo arg,
      <<~EOS
    Hi
  EOS

  # good
  foo arg,
    <<~EOS
      Hi
    EOS
  ~~~

## Regular Expressions

* Prefer plain text search over regular expressions in strings.

  ~~~ ruby
  string["text"]
  ~~~

* Use non-capturing groups when you don't use the captured result.

  ~~~ ruby
  # bad
  /(first|second)/

  # good
  /(?:first|second)/
  ~~~

* Prefer `Regexp#match` over Perl-legacy variables to capture group matches.

  ~~~ ruby
  # bad
  /(regexp)/ =~ string
  process $1

  # good
  /(regexp)/.match(string)[1]
  ~~~

* Prefer named groups over numbered groups.

  ~~~ ruby
  # bad
  /(regexp)/ =~ string
  ...
  process Regexp.last_match(1)

  # good
  /(?<meaningful_var>regexp)/ =~ string
  ...
  process meaningful_var
  ~~~

* Prefer `\A` and `\z` over `^` and `$` when matching strings from start to end.

  ~~~ ruby
  string = "some injection\nusername"
  string[/^username$/] # `^` and `$` matches start and end of lines.
  string[/\Ausername\z/] # `\A` and `\z` matches start and end of strings.
  ~~~

## Percent Literals

* Use `%()` for single-line strings which require both interpolation and
  embedded double-quotes. For multi-line strings, prefer heredocs.

* Avoid `%q` unless you have a string with both `'` and `"` in it. Regular
  string literals are more readable and should be preferred unless a lot of
  characters would have to be escaped in them.

* Use `%r` only for regular expressions matching at least one `/` character.

  ~~~ ruby
  # bad
  %r{\s+}

  # good
  %r{^/(.*)$}
  %r{^/blog/2011/(.*)$}
  ~~~

* Avoid the use of `%s`. Use `:"some string"` to create a symbol with spaces in
  it.

* Prefer `()` as delimiters for all `%` literals, except, as often occurs in
  regular expressions, when parentheses appear inside the literal. Use the first
  of `()`, `{}`, `[]`, `<>` which does not appear inside the literal.

## Testing

* Treat test code like any other code you write. This means: keep readability,
  maintainability, complexity, etc. in mind.

* Prefer Minitest as the test framework.

* Limit each test case to cover a single aspect of your code.

* Organize the setup, action, and assertion sections of the test case into
  paragraphs separated by empty lines.

  ~~~ ruby
  test "sending a password reset email clears the password hash and set a reset token" do
    user = User.create!(email: "bob@example.com")
    user.mark_as_verified

    user.send_password_reset_email

    assert_nil user.password_hash
    refute_nil user.reset_token
  end
  ~~~

* Split complex test cases into multiple simpler tests that test functionality
  in isolation.

* Prefer using `test "foo"`-style syntax to define test cases over `def
  test_foo`.

* Prefer using assertion methods that will yield a more descriptive error
  message.

  ~~~ ruby
  # bad
  assert user.valid?
  assert user.name == "tobi"


  # good
  assert_predicate user, :valid?
  assert_equal "tobi", user.name
  ~~~

* Avoid using `assert_nothing_raised`. Use a positive assertion instead.

* Prefer using assertions over expectations. Expectations lead to more brittle
  tests, especially in combination with singleton objects.

  ~~~ ruby
  # bad
  StatsD.expects(:increment).with("metric")
  do_something

  # good
  assert_statsd_increment("metric") do
    do_something
  end
  ~~~
