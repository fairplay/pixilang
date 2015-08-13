# Embe
## [Pixilang](http://www.warmplace.ru/soft/pixilang/) library for modular audio synthesis


    include "embe.pixi"

    // Create 2 square waveform oscillators, 220Hz (A3) and 330Hz (E4), 
    // amplitude 0.1 (amplitude value from 0 to 1)
    $o1 = osc_sqr(220, 0.1) 
    $o2 = osc_sqr(330, 0.1)

    // Create noise oscillator 
    $o3 = osc_noise(0.5) 

    // Create mixer
    $mix = mix()

    // Join ("j") our oscillators to the mixer
    j($mix, $o1)
    j($mix, $o2)
    j($mix, $o3)

    // Create band-pass filter with cutoff frequency == 110Hz 
    // and resonance value = 100% (if we will use value > 100)
    // filter will start to self-oscillate
    $hz = 110
    $f = flt_bp($hz, 100)
    j($f, $mix)

    // Create visualizer
    $v = vis()

    // Join our filter to visualizer
    j($v, $f)

    // Send our filter to the output
    out($f)

    $d = 1
    while (1) {    
        // Some cutoff modulation
        if TICK > SRATE / 4 {
            TICK - SRATE / 4
            $hz * pow(2, $d / 12)

            if $hz > 880 * 4 {
                $d = -1
            } 

            if ($hz < 110) {
                $d = 1
            }

            $f.Freq = $hz
        }

        // Some visualization
        $v.render($v)
        
        while( get_event() ) { if EVT[ EVT_TYPE ] == EVT_QUIT { halt } }
    }

[Result](https://youtu.be/QpGlRY7oSz0)
