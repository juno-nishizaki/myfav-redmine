FROM redmine:4.1-passenger

COPY config/configuration.yml config

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
        \
# install plugins
        gosu redmine git clone --depth 1 https://github.com/akiko-pusu/redmine_banner.git plugins/redmine_banner; \
        gosu redmine git clone --depth 1 https://github.com/akiko-pusu/redmine_issue_badge.git plugins/redmine_issue_badge; \
        gosu redmine git clone --depth 1 https://github.com/akiko-pusu/redmine_issue_templates.git  plugins/redmine_issue_templates; \ 
        gosu redmine git clone --depth 1 https://github.com/clear-code/redmine_full_text_search.git plugins/full_text_search; \        
        gosu redmine git clone --depth 1 https://github.com/danmunn/redmine_dmsf.git plugins/redmine_dmsf; \
        gosu redmine git clone --depth 1 https://github.com/jbbarth/redmine_base_deface.git plugins/redmine_base_deface; \
        gosu redmine git clone --depth 1 https://github.com/jbbarth/redmine_drafts.git plugins/redmine_drafts; \
        gosu redmine git clone --depth 1 https://github.com/onozaty/redmine-view-customize.git plugins/view_customize; \
        \
# install themes
        gosu redmine git clone -b redmine4.1 --depth 1 https://github.com/farend/redmine_theme_farend_bleuclair.git public/themes/bleuclair; \
        \
# install gems
        gosu redmine bundle install;
