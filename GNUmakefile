all:: bem-mini-offline bem-mini-tools
#all:: bem-bl bem-mini bem-mini-tools
all:: $(patsubst %.bemjson.js,%.html,$(wildcard pages/*/*.bemjson.js))

BEM_BUILD=bem build \
	-l bem-mini-offline/ \
	-l blocks/ \
	-d $< \
	-t $1 \
	-o $(@D) \
	-n $(*F)


#BEM_BUILD=bem build \
#	-l bem-mini/ \
#	-l blocks/ \
#	-d $< \
#	-t $1 \
#	-o $(@D) \
#	-n $(*F)	

BEM_CREATE=bem create block \
		-l pages \
		-t $1 \
		$(*F)

LESS_BUILD= lessc $< $(@D)/$(*F).css

#%.html: %.bemhtml.js %.js %.lessbuild
#	$(call BEM_CREATE,bem-bl/blocks-common/i-bem/bem/techs/html.js)


%.html: %.bemhtml.js %.js %.css %.ie.css
	$(call BEM_CREATE,bem-mini-offline/i-bem/bem/techs/html.js)

%.bemhtml.js: %.deps.js
	$(call BEM_BUILD,bem-mini-offline/i-bem/bem/techs/bemhtml.js)
#	$(call BEM_BUILD,bem-bl/blocks-common/i-bem/bem/techs/bemhtml.js)

%.deps.js: %.bemdecl.js
	$(call BEM_BUILD,deps.js)
	if [ -e $(@D)/$(*F).html ]; then rm $(@D)/$(*F).html; fi
	if [ -e $(@D)/$(*F).css ]; then rm $(@D)/$(*F).css; fi
	if [ -e $(@D)/$(*F).ie.css ]; then rm $(@D)/$(*F).ie.css; fi
	if [ -e $(@D)/$(*F).js ]; then rm $(@D)/$(*F).js; fi
	if [ -e $(@D)/$(*F).styl ]; then rm $(@D)/$(*F).styl; fi
	if [ -e $(@D)/$(*F).ie.styl ]; then rm $(@D)/$(*F).ie.styl; fi

%.bemdecl.js: %.bemjson.js
	$(call BEM_CREATE,bemdecl.js)

.PRECIOUS: %.css
%.css: %.deps.js
	$(call BEM_BUILD,css)
	borschik -t css -i $(@D)/$(*F).css -o $(@D)/_$(*F).css

.PRECIOUS: %.ie.css
%.ie.css: %.deps.js
	$(call BEM_BUILD,ie.css)
	borschik -t css -i $(@D)/$(*F).ie.css -o $(@D)/_$(*F).ie.css

.PRECIOUS: %.ie.less
%.ie.less: %.deps.js
	$(call BEM_BUILD,ie.less)

.PRECIOUS: %.less
%.less: %.deps.js
	$(call BEM_BUILD,less)

.PRECIOUS: %.lessbuild
%.lessbuild: %.less
	$(call LESS_BUILD,less,css)

.PRECIOUS: %.js
%.js: %.deps.js
	$(call BEM_BUILD,js)


DO_GIT=@echo -- git $1 $2; \
	if [ -d $2 ]; \
		then \
			cd $2 && git pull origin master; \
		else \
			git clone $1 $2; \
	fi

bem-bl:
	$(call DO_GIT,git://github.com/bem/bem-bl.git,$@)

bem-mini:
	$(call DO_GIT,git://github.com/banzalik/bem-mini.git,$@)

bem-mini-tools:
	$(call DO_GIT,git://github.com/banzalik/bem-mini-tools.git,$@)

bem-mini-offline:
	$(call DO_GIT,git://github.com/banzalik/bem-mini-offline.git,$@)


.PHONY: all
