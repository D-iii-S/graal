#
# Copyright (c) 2019, 2020, Oracle and/or its affiliates.
#
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without modification, are
# permitted provided that the following conditions are met:
#
# 1. Redistributions of source code must retain the above copyright notice, this list of
# conditions and the following disclaimer.
#
# 2. Redistributions in binary form must reproduce the above copyright notice, this list of
# conditions and the following disclaimer in the documentation and/or other materials provided
# with the distribution.
#
# 3. Neither the name of the copyright holder nor the names of its contributors may be used to
# endorse or promote products derived from this software without specific prior written
# permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" AND ANY EXPRESS
# OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF
# MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT HOLDER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL,
# EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE
# GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED
# AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING
# NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED
# OF THE POSSIBILITY OF SUCH DAMAGE.
#
QUIETLY$(MX_VERBOSE)  = @

default: reload/ref.bc

reload/libA$(SO_EXT): reload/a.c reload/Makefile
	@mkdir -p $(shell dirname $@)
	$(QUIETLY) $(CLANG) $(CPPFLAGS) $(CFLAGS) $(LDFLAGS) $(SO_LDFLAGS) -fPIC -shared -o $@ $<

reload/libB$(SO_EXT): reload/b.c reload/Makefile reload/libC$(SO_EXT)
	@mkdir -p $(shell dirname $@)
	$(QUIETLY) $(CLANG) $(CPPFLAGS) $(CFLAGS) $(LDFLAGS) $(SO_LDFLAGS) -fPIC -shared -rpath $(ORIGIN_TOKEN) -Lreload -lC -o $@ $<

reload/libC$(SO_EXT): reload/c.c reload/Makefile reload/libA$(SO_EXT)
	@mkdir -p $(shell dirname $@)
	$(QUIETLY) $(CLANG) $(CPPFLAGS) $(CFLAGS) $(LDFLAGS) $(SO_LDFLAGS) -fPIC -shared -rpath $(ORIGIN_TOKEN)/../reload -Lreload -lA -o $@ $<

reload/libD$(SO_EXT): reload/libA$(SO_EXT) reload/Makefile
	@rm -rf $@
	$(QUIETLY) ln -s $(notdir $<) $@

reload/ref.out: reload/main.c reload/Makefile reload/libA$(SO_EXT) reload/libB$(SO_EXT) reload/libD$(SO_EXT)
	@mkdir -p $(shell dirname $@)
	$(QUIETLY) $(CLANG) $(CPPFLAGS) $(CFLAGS) $(LDFLAGS) -rpath $(ORIGIN_TOKEN) -Lreload -lB -lD -lA -o $@ $<

%.bc: %.out
	$(QUIETLY) cp $< $@
