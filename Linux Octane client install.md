# Installing the Octane client on Linux

The Octane client install instructions are for Windows, and there are a few differences when using Linux. These are captured here.

Install Cisco Secure Client.

Launch the client with with:
\
gtk-launch com.cisco.secureclient.gui



# Install trusted root CA certificates
Download and unpack certificates.

(sudo) copy the certificates to /usr/local/share/ca-certificates

rename the certificate's .0 extensions to .crt. Then add the certificates with: \
sudo update-ca-certificates

\
\
Connect following the user guide.
