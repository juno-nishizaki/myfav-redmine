FROM redmica/redmica:1.1.0-passenger

COPY --chown=redmine:redmine ./config/configuration.yml config

RUN set -eux; \
        \
        apt-get update && apt-get install -y --no-install-recommends \
                antiword \
                catdoc \
                catdvi \
                djview \
                djview3 \
                gcc \
                gzip \
                libemail-outlook-message-perl \
                libwpd-tools \
                libwps-tools \
                libxapian-dev \
                make \
                poppler-utils \
                ruby-xapian \
                uuid \
                uuid-dev \
                unrtf \
                unzip \
                xapian-omega \
                xpdf \
                xz-utils \
        ; \
        rm -rf /var/lib/apt/lists/*; \
        \
# install plugins
        gosu redmine git clone --depth 1 https://github.com/akiko-pusu/redmine_banner.git plugins/redmine_banner; \
        gosu redmine git clone --depth 1 https://github.com/akiko-pusu/redmine_issue_badge.git plugins/redmine_issue_badge; \
        gosu redmine git clone --depth 1 https://github.com/akiko-pusu/redmine_issue_templates.git  plugins/redmine_issue_templates; \
        gosu redmine git clone --depth 1 https://github.com/bizyman/sidebar_hide.git plugins/sidebar_hide; \
        gosu redmine git clone --depth 1 https://github.com/cat-in-136/redmine_scheduling_poll.git plugins/redmine_scheduling_poll; \
        gosu redmine git clone --depth 1 https://github.com/clear-code/redmine_full_text_search.git plugins/full_text_search; \
        gosu redmine git clone --depth 1 https://github.com/danmunn/redmine_dmsf.git plugins/redmine_dmsf; \
        gosu redmine git clone --depth 1 https://github.com/deecay/redmine_pivot_table.git plugins/redmine_pivot_table; \
        gosu redmine git clone --depth 1 https://github.com/haru/redmine_logs.git plugins/redmine_logs; \
        gosu redmine git clone --depth 1 https://github.com/haru/redmine_theme_changer.git plugins/redmine_theme_changer; \
        gosu redmine git clone --depth 1 https://github.com/haru/redmine_wiki_extensions.git plugins/redmine_wiki_extensions; \
#        gosu redmine git clone --depth 1 https://github.com/hidakatsuya/redmine_default_custom_query.git plugins/redmine_default_custom_query; \
        gosu redmine git clone --depth 1 https://github.com/ishikawa999/redmine_message_customize.git plugins/redmine_message_customize; \
        gosu redmine git clone --depth 1 https://github.com/jbbarth/redmine_base_deface.git plugins/redmine_base_deface; \
        gosu redmine git clone --depth 1 https://github.com/jbbarth/redmine_drafts.git plugins/redmine_drafts; \
        gosu redmine git clone --depth 1 https://github.com/mikitex70/redmine_drawio.git plugins/redmine_drawio; \
        gosu redmine git clone --depth 1 https://github.com/onozaty/redmine-view-customize.git plugins/view_customize; \
        gosu redmine git clone --depth 1 https://github.com/redmica/redmine_issues_panel.git plugins/redmine_issues_panel; \
        gosu redmine git clone --depth 1 https://github.com/Smile-SA/redmine_stealth.git plugins/redmine_stealth; \
        gosu redmine git clone --depth 1 https://github.com/suer/redmine_enter_cancel.git plugins/redmine_enter_cancel; \
        gosu redmine git clone --depth 1 https://github.com/tkusukawa/redmine_work_time.git plugins/redmine_work_time; \
        gosu redmine git clone --depth 1 https://github.com/two-pack/redmine_xlsx_format_issue_exporter.git plugins/redmine_xlsx_format_issue_exporter; \
        \
# install themes
        gosu redmine git clone --depth 1 https://github.com/akiko-pusu/redmine_theme_kodomo.git public/themes/redmine_theme_kodomo; \
        gosu redmine git clone --depth 1 https://github.com/akiko-pusu/redmine_theme_kodomo_midori.git public/themes/redmine_theme_kodomo_midori; \
        gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_basic.git public/themes/farend_basic; \
#        gosu redmine git clone -b redmine4.1 --depth 1 https://github.com/farend/redmine_theme_farend_bleuclair.git public/themes/bleuclair; \
        gosu redmine git clone --depth 1 https://github.com/farend/redmine_theme_farend_fancy.git public/themes/farend_fancy; \
        gosu redmine git clone --depth 1 https://github.com/makotokw/redmine-theme-gitmike.git public/themes/gitmike; \
        \
# install gems
        sed -i -e "s/^.*gem 'rubyzip'.*\$//g" plugins/redmine_dmsf/Gemfile; \
        gosu redmine bundle install;

# Optional:
# [Redmine theme for kids / Kodomo Redmine](https://github.com/akiko-pusu/redmine_theme_kodomo)
# [Redmine theme for kids midori version / Kodomo Redmine green version](https://github.com/akiko-pusu/redmine_theme_kodomo_midori)
# Copy additional fonts if necessary.
COPY --chown=redmine:redmine ./optional/redmine_theme_kodomo /tmp/myfav-redmine-work/
RUN set -eux; \
        \
        if [  -f /tmp/myfav-redmine-work/font/APJapanesefont.ttf \
           -a -f /tmp/myfav-redmine-work/font/APJapanesefontT.ttf ]; then \
                # Redmine theme for kids / Kodomo Redmine
                cp -a /tmp/myfav-redmine-work/font/APJapanesefont.ttf public/themes/redmine_theme_kodomo/font; \
                cp -a /tmp/myfav-redmine-work/font/APJapanesefontT.ttf public/themes/redmine_theme_kodomo/font; \
                sed -i -e "s/'Patrick Hand'/'Patrick Hand', 'anzu'/g" public/themes/redmine_theme_kodomo/stylesheets/fonts.css; \
                cat /tmp/myfav-redmine-work/stylesheets/fonts.css | tee -a public/themes/redmine_theme_kodomo/stylesheets/fonts.css; \

                # Redmine theme for kids midori version / Kodomo Redmine green version
                cp -a /tmp/myfav-redmine-work/font/APJapanesefont.ttf public/themes/redmine_theme_kodomo_midori/font; \
                cp -a /tmp/myfav-redmine-work/font/APJapanesefontT.ttf public/themes/redmine_theme_kodomo_midori/font; \
                sed -i -e "s/'Patrick Hand'/'Patrick Hand', 'anzu'/g" public/themes/redmine_theme_kodomo_midori/stylesheets/fonts.css; \
                cat /tmp/myfav-redmine-work/stylesheets/fonts.css | tee -a public/themes/redmine_theme_kodomo_midori/stylesheets/fonts.css; \
        fi; \
        \
        rm -rf /tmp/myfav-redmine-work;

