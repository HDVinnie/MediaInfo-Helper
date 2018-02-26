[![Codacy Badge](https://api.codacy.com/project/badge/Grade/2cf2367f96904e08b47507cf8331cfa2)](https://www.codacy.com/app/HDVinnie/MediaInfo-Helper?utm_source=github.com&utm_medium=referral&utm_content=HDVinnie/MediaInfo-Helper&utm_campaign=badger)
[![Latest Stable Version](https://poser.pugx.org/hdvinnie/mediainfo-helper/v/stable)](https://packagist.org/packages/hdvinnie/mediainfo-helper) [![Total Downloads](https://poser.pugx.org/hdvinnie/mediainfo-helper/downloads)](https://packagist.org/packages/hdvinnie/mediainfo-helper)

# MediaInfo-Helper

A Laravel helper for parsing MediaInfo dumps.

## Install

Via Composer

``` bash
$ composer require hdvinnie/mediainfo-helper
```

## Usage

**Parsing a MediaInfo string**

Returns an array containing the parsed information.

```php
$parser = new MediaInfo();
$parsed = $parser->parse($mediaInfo);
```

## Example

**In Your Controller**
```php
        $general = null;
        $video = null;
        $settings = null;
        $audio = null;
        $general_crumbs = null;
        $text_crumbs = null;
        $subtitle = null;
        $view_crumbs = null;
        $video_crumbs = null;
        $settings = null;
        $audio_crumbs = null;
        $subtitle = null;
        $subtitle_crumbs = null;
        if ($movie->mediainfo != null) {
            $parser = new MediaInfo();
            $parsed = $parser->parse($movie->mediainfo);
            $view_crumbs = $parser->prepareViewCrumbs($parsed);
            $general = $parsed['general'];
            $general_crumbs = $view_crumbs['general'];
            $video = $parsed['video'];
            $video_crumbs = $view_crumbs['video'];
            $settings = ($parsed['video'] !== null && isset($parsed['video'][0]) && isset($parsed['video'][0]['encoding_settings'])) ? $parsed['video'][0]['encoding_settings'] : null;
            $audio = $parsed['audio'];
            $audio_crumbs = $view_crumbs['audio'];
            $subtitle = $parsed['text'];
            $text_crumbs = $view_crumbs['text'];
        }
```

**In You View**
```html
@section('content')
@if($movie->mediainfo != null)
<div class="table-responsive">
<table class="table table-condensed table-bordered table-striped">
<tbody>
      <tr>
        <td>
          <div class="panel-body">
              <center><span class="text-bold text-blue">@emojione(':blue_heart:') Media Info Output @emojione(':blue_heart:')</span></center>
              <br>
              @if($general !== null && isset($general['file_name']))
                <span class="text-bold text-blue">@emojione(':name_badge:') FILE:</span>
                <span class="text-bold"><em>{{ $general['file_name'] }}</em></span>
                <br>
                <br>
              @endif
              @if($general_crumbs !== null)
                <span class="text-bold text-blue">@emojione(':information_source:') GENERAL:</span>
                <span class="text-bold"><em>
                    @foreach($general_crumbs as $crumb)
                      {{ $crumb }}
                      @if(!$loop->last)
                        /
                      @endif
                    @endforeach
                  </em></span>
                <br>
                <br>
              @endif
              @if($video_crumbs !== null)
                @foreach($video_crumbs as $key => $v)
                  <span class="text-bold text-blue">@emojione(':projector:') VIDEO:</span>
                  <span class="text-bold"><em>
                      @foreach($v as $crumb)
                        {{ $crumb }}
                        @if(!$loop->last)
                          /
                        @endif
                      @endforeach
                    </em></span>
                  <br>
                  <br>
                @endforeach
              @endif
              @if($audio_crumbs !== null)
                @foreach($audio_crumbs as $key => $a)
                <span class="text-bold text-blue">@emojione(':loud_sound:') AUDIO {{ ++$key }}:</span>
                <span class="text-bold"><em>
                    @foreach($a as $crumb)
                      {{ $crumb }}
                      @if(!$loop->last)
                        /
                      @endif
                    @endforeach
                  </em></span>
                <br>
                @endforeach
              @endif
              <br>
              @if($text_crumbs !== null)
                @foreach($text_crumbs as $key => $s)
                <span class="text-bold text-blue">@emojione(':speech_balloon:') SUBTITLE {{ ++$key }}:</span>
                <span class="text-bold"><em>
                    @foreach($s as $crumb)
                        {{ $crumb }}
                        @if(!$loop->last)
                          /
                      @endif
                    @endforeach
                  </em></span>
                <br>
                @endforeach
              @endif
              @if($settings)
              <br>
              <span class="text-bold text-blue">@emojione(':gear:') ENCODE SETTINGS:</span>
              <br>
              <div class="decoda-code text-black">{{ $settings }}</div>
              @endif
              <br>
              <br>
              <center>
              <button class="show_hide btn btn-labeled btn-primary" href="#">
                <span class="btn-label">@emojione(':poop:')</span>Show/Hide Original Dump</button>
              </center>
              <div class="slidingDiv">
                <pre class="decoda-code"><code>{{ $movie->mediainfo }}</code></pre>
            </div>
          </div>
        </td>
      </tr>
    </tbody>
  </table>
</div>
@endif
@endsection

@section('javascripts')
<script>
$(document).ready(function(){

$(".slidingDiv").hide();
$(".show_hide").show();

$('.show_hide').click(function(){
$(".slidingDiv").slideToggle();
});

});
</script>
@endsection
```



## Testing

``` bash
$ composer test
```

## License
[The MIT License](LICENSE)
