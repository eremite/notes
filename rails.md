# Notes on rails

## Paste into console to pretend to download all uploaded iamges
    site_path = "http://www.es2eng.com"
    [
      [Photo, :image],
    ].each do |klass, method|
      bases = {}
      FileUtils.rm_rf "#{Rails.root}/public/system/#{klass.to_s.underscore.pluralize}"
      klass.all.each do |model|
        attachment = model.send(method)
        next unless attachment.present?
        attachment.styles.keys.each do |style|
          FileUtils.mkdir_p File.dirname(attachment.path(style))
          if bases[style].nil?
            bases[style] = "#{Rails.root}/public/system/#{klass.to_s.underscore.pluralize}/#{style}.jpg"
            `wget #{site_path}#{attachment.url(style)} -O #{bases[style]}`
          end
          FileUtils.ln_s bases[style], attachment.path(style)
        end
      end
    end
