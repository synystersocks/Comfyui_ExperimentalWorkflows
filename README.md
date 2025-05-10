# Comfyui_ExperimentalWorkflows
This Repo contains experimental Comfyui workflows, please feel free to try and test these out :)

------------------------------
Update 1 ---

Gimm Frame Interpolators,
 
2x universal in/out,
4x universal in/out,
*some nice and basic workflows, that do the job nicely :).

Skyreel i2v Quad, Triple Ksampler Segmentation,

outputs 4 videos at 16fps/97frames @ 960px / 544px "total process time = 30 minuites on rtx 3060ti 8gb",
stabilized using recursive teacache, riflex (latent interpolation), and segmented layerskip of L8 & L9 @0.2% start.

each samplerset is split into 3 segments, 
Seg1 = 0%-33.3%, 
Seg2 = 33.3%-66.6%, 
Seg3 = 66.6%-100%.

seg1, should be set as steps - (0 to 20 of 60steps),
seg2, should be set as steps - (for speed = 10 to 20 of 30steps), (for quality = 20 to 40 of 60steps),
seg3, should be set as steps - (for speed = 20 to 30 of 30steps), (for quality = 40 to 60 of 60steps),

Riflex is used to blend the interpolated motion back onto the latents, insted of being used to stabilize higher frame generations.
Riflex can be set at 2, other values higher will reduce movement slighly on increasing "isnt overall a bad thing"

*also about to test reinterpolating the latents back onto the interpolated latents, like a 2pass interpolation blend to correct for outlier values (jankyness).

LayerSkip split as 2 segments for each layer partially skipped (different values are used when skipping both in the same node, this also allows to test other offset values "0.15 and 0.2 are decent values for both")
each should be set the same value, they seem to compliment each other in this way.

Teacache,
with a low 0.1% and low coefficient, across 20steps, equates to 2steps being processed per step, near lossless in quality while halfing the time to process.
this effectivly means your processing is equivilent to 10steps per 20steps, this also scales, 
(larger step ammounts per ksamp stage cause's unnecessary processing of outlier tensor values)

note 1
(These workflows currently use the bf16 of the skyreeli2v1.3b and umt5xxl "gguf", this is to maximise quality of precision weights)
note 2
(if swapping to a 13b/14b model, i would recomment the q8 gguf, maximise the quality of the weights "within limitations")
note 3
(for wan i2v 14b, frames should be set too = 81, and resolution to = 720px / 544px, Teacache coefficient = 0.15.) 
the rest of the workflow will work fine "takes around 2 hours to process all 4 outputs"

------------------------------
