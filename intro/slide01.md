!SLIDE center subsection

# Good practices to help when upgrading the Ruby version in an application

## Fabio Perrella

### Tech Leader @ Locaweb

!SLIDE center

# Why upgrade Ruby version? (in an application)

!SLIDE center

# Why upgrade Ruby?

* Every christmas a new (minor) version of Ruby is released
* Every march (or february) the last supported minor version reaches the end-of-life (eol)

!SLIDE center

# Last Ruby releases

    Ruby 2.7
    status: preview
    release date:

    Ruby 2.6
    status: normal maintenance
    release date: 2018-12-25

    Ruby 2.5
    status: normal maintenance
    release date: 2017-12-25

    Ruby 2.4
    status: security maintenance
    release date: 2016-12-25
    EOL date: 2020-03-31

!SLIDE center

# What about Rails??

## Rails has a release frequency slower than Ruby

## I will not talk about upgrading Rails in this talk (maybe next year!)

!SLIDE center

# Upgrading Ruby

## Imagine a perfect world when you upgrade the Ruby version and all the tests pass!

!SLIDE center red big

# Now, wake up!!

!SLIDE center

# 1st problem: **bundle install**

!SLIDE center

# 1st problem: **bundle install**

* Some gems only work in specific range of Ruby versions
* Some gems have dependencies which works only in specific range of Ruby versions
* Some gems do not know they only work in specific range of Ruby versions!

!SLIDE center

# How a gem specifies the required Ruby version

    @@@ruby
    # This gem will work with 1.8.6 or greater...
    spec.required_ruby_version = '>= 1.8.6'

    # Only with ruby 2.0.x
    spec.required_ruby_version = '~> 2.0'

    # Only prereleases or final releases after 2.6.0.preview2
    spec.required_ruby_version = '> 2.6.0.preview2'

https://guides.rubygems.org/specification-reference/#required_ruby_version

!SLIDE center

# 1st problem: **bundle install**

## Always try to upgrade to the latest Ruby version

Gems are not used to restrict the new versions, examples:

    @@@text
    sinatra.gemspec:  s.required_ruby_version = '>= 2.2.0'
    webmock.gemspec:  s.required_ruby_version = '>= 2.0'
    public_suffix.gemspec:  s.required_ruby_version = ">= 2.1"
    rack.gemspec:  s.required_ruby_version = '>= 2.2.2'
    safe_yaml.gemspec:  gem.required_ruby_version = ">= 1.8.7"
    mustermann.gemspec:  s.required_ruby_version = '>= 2.2.0'
    tzinfo.gemspec:  s.required_ruby_version = '>= 1.8.7'

Upgrading from 2.3 to 2.6 should be easier than upgrading from 2.3 to 2.4

!SLIDE center

# 1st problem: **bundle install**

## Delete your Gemfile.lock, update the .ruby-version and install the gems again

Before do it, it is good to specify all versions with `'~> x.y'` in `Gemfile`, ex:

    @@@ruby
    # example of a gem without version specification in Gemfile
    gem 'aasm'

    # $ bundle show aasm
    # .../lib/ruby/gems/2.6.0/gems/aasm-4.12.3

    # Now, add the version specification to "block" the major upgrade:
    gem 'aasm', '~> 4.12'

By doing this, it prevents to use a new major version which may break some tests

!SLIDE center

# Semver (Semantic Versioning)

https://semver.org

    @@@text
    Given a version number MAJOR.MINOR.PATCH, increment the:

    MAJOR version when you make incompatible API changes,
    MINOR version when you add functionality in a backwards compatible manner, and
    PATCH version when you make backwards compatible bug fixes.

**note1**: Ruby versions **do not** follow Semver, but the majority of the gems
do!

**note2**: If necessary to upgrade a major version, search for files like
CHANGELOG.md or RELEASES.md in gem's source to know why the
compatibility was broken.

**note3**: If you are a gem maintainer, https://keepachangelog.com and follow
Semver!

!SLIDE

- atualização do rails fica pra depois
- problem 1.1 internal gems may support different versions of ruby
- corrija todas as gems internas para suportar o ruby novo, e se possivel o
velho ao mesmo tempo. Com isso, da pra seguir com as atualizações nas 2 versões
    + deletar Gemfile.lock das gems internas
    + configurar pipeline para rodar build com todas as versões suportadas (matriz de versões)
    + se houver IF ruby_version no gemspec, usar o nome do ruby pra indicar como foi o build, ex: r13.0.0.ruby26
- dependencias externas podem atrapalhar na evolução, por exemplo versão do banco
de dados ou do redis
- algumas gems podem estar amarradas a versoes especificas de outras gems
```
-  s.add_runtime_dependency 'activesupport', '~> 5.1.4'
+  s.add_runtime_dependency 'activesupport', '~> 3.2'
```
- problem 2: fazer os testes passarem
- arrume os warnings, ex:
```
-        expect(WebMock).to_not have_requested(:post, %r|lala/new?*|)
+        expect(WebMock).to_not have_requested(:post, %r|lala/new\?*|)
```
- problema 3: fazer o deploy funcionar


!SLIDE

# Finish him! Questions??

## Picks

The source of this presentation: https://github.com/fabioperrella/debugging-with-mastery

This presentation was made with the gem **Showoff**: https://github.com/puppetlabs/showoff

How to find a subject to do a presentation: http://www.greaterthancode.com/2016/11/21/008-sandi-metz-and-katrina-owen/

## Me

https://fabioperrella.github.io

https://github.com/fabioperrella

http://twitter.com/fabioperrella

## Work at Locaweb

https://www.locaweb.com.br/carreira
