# MIKROTELEMANJETKA
Final project for the Building AI course

## Summary

A wristwatch is used to conveniently measure arterial blood pressure, hypertension, hypotension, as well as the heart's systolic and diastolic pressure, rhythm, and pulse.

## Background

High blood pressure is one of those problems that quietly builds up in the background until it suddenly isn't quiet anymore. A huge number of people walk around with hypertension or hypotension for years without knowing it, simply because checking blood pressure properly requires a cuff, a few minutes of sitting still, and honestly, remembering to do it in the first place. Most people just don't.

* Blood pressure problems (both high and low) often show no symptoms until they cause a real health event, like a stroke or heart attack
* Traditional blood pressure monitors are bulky, inconvenient, and not something people carry around or use daily
* Irregular heart rhythm can go unnoticed for a long time if there's no continuous monitoring
* People at risk (older adults, people with a family history of heart disease, athletes pushing their limits) rarely get real-time feedback about what's happening with their heart

My motivation for this project comes from seeing how often people close to me only find out about a blood pressure issue after it's already become serious. A device that's already on your wrist, that you don't have to think about, feels like a much more realistic way to catch these problems early. I also find the technical side genuinely interesting — figuring out how to extract something as complex as blood pressure from a signal you can measure through skin is a real challenge, and that's part of why I wanted to dig into it for this course.

## How is it used?

The idea is simple: you wear it like a regular smartwatch, and it works quietly in the background.

* The watch sits on the wrist and continuously reads signals through its built-in sensors (optical pulse sensor, and ideally a pressure sensor for more accurate readings)
* At set intervals, or whenever the user requests it, the device estimates blood pressure, pulse, and rhythm, and shows the results on the watch screen or through a companion mobile app
* If the AI model detects something unusual — a spike in blood pressure, an irregular rhythm, or values that fall outside the user's normal range — it sends an alert so the person (or a family member, or their doctor) can react in time
* It's meant to be used all day, not just during a doctor's visit: at rest, during light activity, and while sleeping, since blood pressure naturally shifts throughout the day and catching those patterns is part of the value

The target users are people managing hypertension or hypotension, older adults who want peace of mind, people with a family history of cardiovascular issues, and even athletes who want to keep an eye on how their heart responds to training. It also needs to work for people who aren't tech-savvy, so the interface has to stay simple — a glance at the screen should be enough to understand if everything is fine.

## Data sources and AI methods

Blood pressure isn't something you can read directly off skin the way a thermometer reads temperature, so the approach relies on a sensor signal plus a fairly basic machine learning model on top of it — nothing too fancy is needed to get started.

* The core signal comes from PPG (photoplethysmography) — the same kind of optical sensor already used in most fitness trackers to measure pulse, which detects blood volume changes under the skin using light
* For training data, public datasets like **PhysioNet**, which contain PPG signals paired with real blood pressure readings, would be used instead of collecting everything from scratch
* A few simple features are pulled out of each pulse wave (things like how tall the peak is, how long each beat takes, how quickly the signal rises and falls), rather than feeding the whole raw waveform into the model
* Those features are then fed into a simple regression model — something like linear regression or a basic decision tree — to estimate systolic and diastolic blood pressure
* Heart rhythm is checked in an even simpler way: just measuring the time between beats and flagging it if the pattern looks irregular

| Data type | Source | Method |
| --- | --- | --- |
| PPG signal | Watch sensor | Basic signal cleanup |
| Pulse wave features | Extracted from PPG signal | Simple regression model |
| Heart rhythm | Watch sensor (time between beats) | Rule-based check |

## Challenges

This isn't a replacement for a real medical device, and it's important to be upfront about that.

* PPG-based blood pressure estimation is inherently less accurate than a cuff-based measurement, especially for people with irregular skin tone, poor circulation, or during movement
* The device isn't clinically certified, so it shouldn't be used as the sole basis for medical decisions — it's a monitoring and early-warning tool, not a diagnostic one
* False alerts could cause unnecessary anxiety, while missed detections could give someone a false sense of security — both are real risks with this kind of technology
* Continuous health monitoring raises privacy concerns: this data is deeply personal, and it needs to be stored and transmitted securely, with the user fully in control of who else (if anyone) sees it
* The model's accuracy depends heavily on the diversity of the training data — if the datasets used don't represent a wide range of ages, skin tones, and health conditions, the predictions could be biased or unreliable for underrepresented groups

## What next?

There's a lot of room for this to grow beyond a course project.

* Adding ECG capability alongside PPG would significantly improve the accuracy of both blood pressure and rhythm detection
* Building a companion app that lets users share their trends directly with their doctor, rather than just seeing raw numbers on a watch face
* Running a proper clinical validation study to see how the model performs against real cuff measurements across a diverse group of people
* Personalizing the model over time — calibrating it to each individual user's baseline instead of relying on a one-size-fits-all prediction

To get there, I'd need help from people with real hardware/sensor experience, access to a larger and more diverse labeled dataset, and ideally some guidance from someone with a clinical or biomedical background to make sure the approach stays medically sound.

## Acknowledgments

* This project was inspired by the growing use of wearable health tech and the gap between what's technically possible and what's actually accessible to everyday people
* Publicly available datasets such as **PhysioNet** and **MIMIC-III** made it possible to think through an approach without needing to collect clinical data from scratch
* Thanks to the Building AI course for the structure and guidance that shaped how this idea was put together
