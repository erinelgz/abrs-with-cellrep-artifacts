# abrs-with-cellrep-artifacts

# Abstract

To setup a testbed for our ABR evaluation, we used open-source
platforms such as Puffer, Mahimahi, and CellReplay. To be able
to run this testbed, we had to make some modifications to their
source codes in our Ubuntu 24.04 environment. To build Puffer,
we had to resolve the uint64 errors in several files, which can be
easily fixed by adding <cstdint> library on top of the files that
produce errors during the make process. In opus_encoder.cc file,
av_register_all() call is deprecated, so we removed it. Similarly,
we also had to resolve the uint64 errors in Mahimahi and Cell-
Replay. To install some of the dependencies that is mentioned in
Puffer’s documentation, such as Django, we set up a virtual environ-
ment and used it when running the web servers. Running Pensieve
requires Python 2.7 and TensorFlow, which may not be supported
in current systems. We solved this issue by using a Conda environ-
ment. Lastly, CellReplay triggered an iptables error, which can
be solved by switching to legacy iptables. In the nat.cc file, we
undefined iptables and redefined it to use the legacy version.

# Reproducibility

Accessing the artifact(s) Our artifacts are open-source, and
publicly available on GitHub (Puffer, Mahimahi, and CellReplay).
The ABR algorithms that we used are also included in Puffer’s repos-
itory. They can be found in the "abr" directory, and models such
as Pensieve, Gelato, and TTP can be accessed in the "third_party"
directory. Users can clone each project repository, apply the patches
described in A.1, and configure each artifact following their docu-
mentation.
Experiment Workflow To conduct an experiment, the user has
to initialize Puffer’s web client and media server, and in another
terminal start a Mahimahi or CellReplay shell using the desired trace
files. Inside the emulated namespace, launch a headless browser
instance and connect to the client to start a streaming session using
an automated script. In a separate terminal, use tcpdump to capture
packets on port 50001, which is the default port where Puffer sends
video segments from the media server to the web client. After the
test run, export Puffer’s logs and the captured TCP packets for
analysis. Our methodology is explained in more detail in Section 3.
Evaluation and Expected Result When comparing emulators,
we did expect CellReplay to perform better due to its advanced
design compared to Mahimahi. We observed some changes in RTT
and delivery rate between emulators, but we also expected the SSIM
and BufRatio rankings to change slightly. Instead, they remained
constant with only a slight improvement when CellReplay is used.
SSIM and BufRatio patterns for each ABR algorithm were nearly
identical in Mahimahi and CellReplay when the same trace was
used. There were slight deviations in the patterns that became clear
when we took the mean values for each metric across all test runs.
Experiment Customization For our ABR algorithm configura-
tions, we used cubic congestion control algorithm, which is default
in Ubuntu, but if desired, users can also use bbr, by modifying set-
tings.yml file. In addition, we used the recommended configuration
for each ABR algorithm, but for custom experiments, these config-
urations can be modified.
